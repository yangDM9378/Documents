# Data Fetching

- next.js app Router는 새로운 쉬운 data fetching 시스템을 리엑트와 웹 플랫폼에서 빌드하는 방법이 있다.

1. 서버 컴퍼넌트를 사용하여 데이터를 가져온다.
2. 병렬적인 데이터 패칭은 로딩 시간을 줄인다.
3. 레이아웃과 페이지를 위해 패칭 데이터는 가져온다. next.js는 자동으로 중복 요구를 처리한다.
4. 로딩UI와 Streaming과 Suspense는 점진적 페이지 렌더하고 컨텐츠가 로드되는 동안 결과를 표시한다.

## The Fetch() API

- 새로운 데이터 패칭 시스템은 fetch() 웹 API 위에 빌드되고 서버 컴퍼넌트에서 async와 await을 사용한다.
  - 리엑트는 자동으로 중복 요청을 fetch로 확장한다.
  - next.js는 fetch 옵션 caching과 revalidating 규칙으로 각 요구를 허락한다.

## Fetching Data on the Server

- 언제든지, 서버컴퍼넌트에서 패칭 데이터를 추천한다. 서버컴퍼넌트는 서버에서 항상 데이터를 패칭한다.
  - 백엔드의 데이터 자원에 직접 접근을 가진다.
  - access token과 API key 같은 클라이언트에 노출되는 것으로 부터 개인정보에 대해 보안을 더 가진다.
  - 같은 환경에서 데이터를 가져오고 랜더한다. 클라이언트의 메인 쓰레드 일을 더 잘 하게 클라이언트와 서버 사이에 커뮤니케이션을 줄인다.
  - 클라이언트에서 개별 요청 대신 단일로 여러 데이터를 가져오기를 수행한다.
  - 클라이언트와 서버 사이의 waterfalls를 줄인다.
  - 데이터가 원본에 가깝게 발생하여 region에 따라 발생하여 대기 시간이 줄고 성능이 향상된다.

## Fetching Data at the Component Level

- app router에서 layout, page, components에서 데이터 패칭을 할수 있다.
- 데이터 패칭은 Streaming과 Suspense와 호환 가능하다.

## Parallel and Sequential Data Fetching

- 컴포넌트에서 데이터를 패칭할 때 병렬과 순차 두 데이터 패칭 패턴이 필요하다.
- parallel data fetching은 경로의 요청이 즉시 시작되고 동시에 데이터를 로드한다. 로드하는 총 시간이 줄어드는 장점이 있다.
- sequential data fetching은 경로의 요청이 각각 다른 waterfall을 만든다. 로드시간이 길어질 수도 있다.

## Automatic fetch() Request Deduping

- 만일 다양한 컴포넌트에서 같은 데이터가 필요하다면 자동으로 임시적인 cache에 있는 같은 fetch 요구를 캐쉬 할것이다.
- 서버에서, 캐쉬의 생존시간은 서버 요구로 부터 프로세스 완료 랜더까지 남는다.
  - 최적화는 레이아웃, 페이지, 서버, 컴포넌트, generateMetadata 및 generateStaticParam에서 get 요청이 적용된다.
  - 최적화는 static generation동안 적용된다.
- 클라이언트에서 세션의 캐쉬 중복은 모든 페이지가 리로딩 되기 전에 지속된다.
- post 요청은 자동 캐쉬되지 않는다.
- fetch를 사용하지 않으려면 요청 기간 동안 데이터를 수동할 수 있게 기능을 사용해야한다.

## Static and Dynamic Data Fetching

- Static Data는 종종 바뀌지 않는 데이터이다.
- Dynamic 데이터는 유저에 의해 변경되는 데이터이다.
- next.js는 기본값으로 static fetch한다.
- 데이터는 각 요구에 빌드 시간, 캐쉬, 재사용할 것이다. 개발자는 static data의 캐시와 재확인을 컨트롤 해야한다.
- static data 사용에 이점
  - DB에서의 로드 시간을 줄인다.
  - 자동 캐쉬 향상된 로딩 퍼포먼스를 준다.
- 유저 맞춤형 데이터는 지난 데이터를 항상 최신 데이터를 가져오려면 동적 표시하고 캐싱 없이 각 요청 데이터를 가져와야한다.

## Caching Data

- 캐싱은 특정 위치에 데이터를 저장하는 프로세스로 요청 시 원재 소스에서 다시 가져오지 않는 것이다.
- next.js 캐시는 전역적으로 배포할수 있는 영구 HTTP 캐쉬이다.
- next.js는 서버의 요청이 자체 영구 캐싱 동작을 설정할 수 있도록 fetch() 함수의 옵션을 확장한다.
- 컴포넌트 레벨 데이터 패칭과 함께 즉각적인 어플리케이션 코드는 캐싱한다.
- 서버 렌더 되는 동안, 패칭이 지나는 동안 데이터가 이미 사용가능한지 체크한다. 만일 그렇다면 cache된 데이터를 리턴하고 그렇지 않다면 패치와 미래 데이터를 저장한다.

## Revalidating Data

- 유효성 검사와 캐싱과 리패칭된 데이터를 가져오는 것이다.
- 데이터가 변경되거나 어플리케이션에서 지난 데이터를 재빌드 없이 최신 버전으로 표기하기 위해 사용한다.
- Background와 On-demand가 있다.

### Streaming and Suspense

- 진척적인 랜더와 향상된 stream 랜더를 새로운 리엑트 특징이다.
- 서버 컴포넌트와 중첩 레이아웃을 사용하면 특별히 데이터가 필요한 경우가 아니라면 즉시 렌더링하고 데이터를 가져오는 부분에서 로딩 상태를 표시한다.

# Data Fetching

## 서버 컴포넌트에서 async와 await

- 타입스크립트에서 사용시 {/_ @ts-expect-error Async Server Component _/} 적용해야한다.

```
async function getData() {
  const res = await fetch('https://api.example.com/...');
  // The return value is *not* serialized
  // You can return Date, Map, Set, etc.

  // Recommendation: handle errors
  if (!res.ok) {
    // This will activate the closest `error.js` Error Boundary
    throw new Error('Failed to fetch data');
  }

  return res.json();
}

export default async function Page() {
  const data = await getData();

  return <main></main>;
}
```

## 클라이언트 컴포넌트에서 use

- await과 유사한 promise를 받는다.
- Wrapping fetch은 클라이언트 컴포넌트에서 추천되지 않으며 다양한 re-render를 트리거할 수 잇다.
- 클라이언트에서 fetch 데이터가 필요하다면 SWR 또는 ReactQuery를 추천한다.

## Static Data Fetching

- fetch는 자동으로 캐시 할 것이다.

```
fetch('https://...'); // cache: 'force-cache' is the default
```

### Revalidating Data

- revalidata 캐시 데이터 시간 간격이 있다. next.revalidata 옵션을 통해 fetch()에서 cache 수명주기를 세팅 할 수 있다.

```
fetch('https://...', { next: { revalidate: 10 } });
```

### Dynamic Data Fetching

```
fetch('https://...', { cache: 'no-store' });
```

## Data Fetching Patterns

### Parallel Data Fetching

- 클라이언트와 서버 waterfall을 줄이기 위해 병렬적 pattern을 추천한다.

```
import Albums from './albums';

async function getArtist(username: string) {
  const res = await fetch(`https://api.example.com/artist/${username}`);
  return res.json();
}

async function getArtistAlbums(username: string) {
  const res = await fetch(`https://api.example.com/artist/${username}/albums`);
  return res.json();
}

export default async function Page({
  params: { username },
}: {
  params: { username: string };
}) {
  // Initiate both requests in parallel
  const artistData = getArtist(username);
  const albumsData = getArtistAlbums(username);

  // Wait for the promises to resolve
  const [artist, albums] = await Promise.all([artistData, albumsData]);

  return (
    <>
      <h1>{artist.name}</h1>
      <Albums list={albums}></Albums>
    </>
  );
}
```

- 사용자 경험 개선을 위해 서스펜스 경계를 추가하여 렌더링 작업을 중단하고 결과를 일부 표시할 수 있다.

```
import { getArtist, getArtistAlbums, type Album } from './api';

export default async function Page({
  params: { username },
}: {
  params: { username: string };
}) {
  // Initiate both requests in parallel
  const artistData = getArtist(username);
  const albumData = getArtistAlbums(username);

  // Wait for the artist's promise to resolve first
  const artist = await artistData;

  return (
    <>
      <h1>{artist.name}</h1>
      {/* Send the artist information first,
          and wrap albums in a suspense boundary */}
      <Suspense fallback={<div>Loading...</div>}>
        {/* @ts-expect-error Async Server Component */}
        <Albums promise={albumData} />
      </Suspense>
    </>
  );
}

// Albums Component
async function Albums({ promise }: { promise: Promise<Album[]> }) {
  // Wait for the albums promise to resolve
  const albums = await promise;

  return (
    <ul>
      {albums.map((album) => (
        <li key={album.id}>{album.name}</li>
      ))}
    </ul>
  );
}
```

### Sequential Data Fetching

```
async function Playlists({ artistID }: { artistID: string }) {
  // Wait for the playlists
  const playlists = await getArtistPlaylists(artistID);

  return (
    <ul>
      {playlists.map((playlist) => (
        <li key={playlist.id}>{playlist.name}</li>
      ))}
    </ul>
  );
}

export default async function Page({
  params: { username },
}: {
  params: { username: string };
}) {
  // Wait for the artist
  const artist = await getArtist(username);

  return (
    <>
      <h1>{artist.name}</h1>
      <Suspense fallback={<div>Loading...</div>}>
        {/* @ts-expect-error Async Server Component */}
        <Playlists artistID={artist.id} />
      </Suspense>
    </>
  );
}
```

### route에 Blocking Rendering

- 데이터를 가져오면 모든 경로 세그먼트에 대한 렌더링은 데이터 로드 완료 후 시작 가능하다.

1. loading 사용하여 표시 할 수 있다.
2. 특정 부분만 차단하도록 구성요소에서 적용가능하다.

# Caching Data

- 요청별 전체 경로 세그먼트에 대해 데이터 캐싱을 기본 지원한다.

## Per-request Caching

### fetch()

- fetch()는 자동 중복 제거와 cach를 요구한다.

```
async function getComments() {
  const res = await fetch('https://...'); // The result is cached
  return res.json();
}

// This function is called twice, but the result is only fetched once
const comments = await getComments(); // cache MISS

// The second call could be anywhere in your application
const comments = await getComments(); // cache HIT
```

- 요구가 캐싱 되지 않는 경우
  - Dynamic methods사용과 post 요구
  - fetchCache는 cache를 스킵
  - revalidate: 0 또는 cache: 'no-stroe'

```
export default async function Page() {
  // revalidate this data every 10 seconds at most
  const res = await fetch('https://...', { next: { revalidate: 10 } });
  const data = res.json();
  // ...
}
```

### React cache()

- 래핑된 함수 호출의 결과를 메모하며 요청을 cache() 및 중복 제거한다.
- getUser() 함수를 두번 호출해도 데이터베이스 쿼리는 한 번만 수행된다.
- getUser()가 cache()에 래핑되어 두 번째 요청이 첫 요청의 결과를 재사용 가능 하기 때문이다.

```
# utils/getUser.ts
import { cache } from 'react';

export const getUser = cache(async (id: string) => {
  const user = await db.user.findUnique({ id });
  return user;
});
```

```
# app/user/[id]/layout.tsx
import { getUser } from '@utils/getUser';

export default async function Page({
  params: { id },
}: {
  params: { id: string };
}) {
  const user = await getUser(id);
  // ...
}
```

### cache()와 Preload pattern

- preload()라는 옵션으로 선택적 노출한다.

```
import { getUser } from '@utils/getUser';

export const preload = (id: string) => {
  // void evaluates the given expression and returns undefined
  // https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/void
  void getUser(id);
};
export default async function User({ id }: { id: string }) {
  const result = await getUser(id);
  // ...
}
```

### cache, preload, server-only 혼합

```
import { cache } from 'react';
import 'server-only';

export const preload = (id: string) => {
  void getUser(id);
};

export const getUser = cache(async (id: string) => {
  // ...
});
```

## Segment-level Caching

- 경로 세그먼트에 사용된 데이터를 캐시하고 재검증 한다.
- 경로의 여러 세그먼트가 전체 경로의 캐시 수명을 제어 하며 재검증 시간 설정하는 재검증 값을 내보낼 수 있다.

```
export const revalidate = 60; // revalidate this segment every 60 seconds
```

# Revalidating Data

- next.js는 리빌드 없이 전체 사이트를 특별히 static routes를 가능하게 한다.
- revalidation은 Incremental Static Regeneration로 알려져 있다.
- revalidation은 수백만의 페이지를 스케일 동안에 이익을 유지한다.
- background와 on-demand 타입 두개가 있다.

## Background Revalidation

- 특별한 간격의 캐시된 데이터를 fetch()에서 리소스 수명을 조절하기 위해 next.revalidate 옵션을 사용하여 재검증 한다.

```
fetch('https://...', { next: { revalidate: 60 } });
```

- fetch를 사용하지 않고 경로 세그먼트를 사용하여 revalidate data를 원한다면 외부 패키지나 query 빌더를 사용해야한다.

```
export const revalidate = 60; // revalidate this page every 60 seconds
```

- fetch를 추가하여 revalidate 데이터 사용을 cache할 수 있다.

### 사용방법

1. 빌드시 정적 렌더링 경로에 대한 요청 시 캐시된 데이터 표시합니다.
2. 초기 요청 이후와 60초 이전의 경로에 대한 요청도 캐시 되고 즉각적이다.
3. 60초 기간이 지나면 다음 요청에 캐시된 데이터가 표시된다.
4. next.js는 background에서 데이터를 트리거 한다.
5. 경로가 생성되면 캐시를 무효화하고 업데이터된 경로를 표시한다. 경로 생성 실패시 변경 되지 않는다

## On-demand Revalidation

- 60초의 revalidate를 세팅하면 1분동안 모든 방문자들은 같은 버전을 볼것이다.
- 60초가 지난후에 페이지를 방문하면 새로운 cache된 페이지를 볼 것이다.
- next.js app router는 revalidate된 내용을 route 또는 cache 태그를 기초로 공급한다.
- 특별한 fetch를 위해 next.js 캐쉬를 수동적으로 제거하고 사이트를 쉽게 업데이트 가능하다.
  - headless CMS의 내용이 새애성되거나 업데이트 된다.
  - 전자 상거래 메타데이터 변경

### On-Demand Revalidation

- 데이터는 revalidatePath 경로로 부터 on-demand 또는 revalidateTag 캐시로 부터 revalidated 할 수 있다.
- fetch에 cache tag collection 추가하는 예제이다.

```
export default async function Page() {
  const res = await fetch('https://...', { next: { tags: ['collection'] } });
  const data = await res.json();
  // ...
}
```

- chach된 데이터는 라우터 핸들러에 revalidateTag로 부터 on-demand revalidate 할 수 있다.

```
import { NextRequest, NextResponse } from 'next/server';
import { revalidateTag } from 'next/cache';

export async function GET(request: NextRequest) {
  const tag = request.nextUrl.searchParams.get('tag');
  revalidateTag(tag);
  return NextResponse.json({ revalidated: true, now: Date.now() });
}
```

## Error Handing and Revalidation

- revalidate data에 시도하는 동안 오류 발생하면 마지막 성공 생성 데이터가 제공된다.
- 다음 요청에서 재검증을 재시도 한다.

# Server Actions

- Server Actions는 React Actions 위에 구축된 기능이다.
- 서버측 데이터 변경, 클라이언 측 JS 감소 및 향상된 양식을 가능케 한다.

```
import { cookies } from 'next/headers';

export default function AddToCart({ productId }) {
  async function addItem(data) {
    'use server';

    const cartId = cookies().get('cartId')?.value;
    await saveToDb({ cartId, data });
  }

  return (
    <form action={addItem}>
      <button type="submit">Add to Cart</button>
    </form>
  );
}
```

## Convention

```
# next.config.js
module.exports = {
  experimental: {
    serverActions: true,
  },
};
```

### Creation

- 'user server'로 비동기 함수를 정의하여 서버 액션을 생성

```
async function myAction() {
  'use server';
  // ...
}
```

- 파일 최상위에 'use server' 지시문을 통해 클라이언트 구성 요소에서 서버 작업을 가져요는 경우 사용

```
'use server';

export async function myAction() {
  // ...
}
```

### 사용법

- Using action: 리엑트 action은 `<form>` 요소에서 Server Action 호출 가능하다.
- Using fromAction: 리엑트 formAction은 `<form>`서 `<button>`, `<input type='submit'>`, `<input type='image'>` 핸들링을 허락한다.
- StartTransition을 사용한 지정 호출: startTransition을 사용하여 action 또는 formAction을 사용하지 않고 서버 작업을 호출한다. Progressive Enhancement를 비활성화 한다.
- action
  - form 요소에서 React의 action은 Server Action 호출은 action prop과 함께 전달한 Server Action은 사용자 상호 작용에 대한 응답으로 비동기 작동한다.

```
export default function AddToCart({ productId }) {
  async function addItem(data) {
    'use server';

    const cartId = cookies().get('cartId')?.value;
    await saveToDb({ cartId, data });
  }

  return (
    <form action={addItem}>
      <button type="submit">Add to Cart</button>
    </form>
  );
}
```

- form action
  - Form Action에서 button요소, input type='submit', input type='image'는 핸들링 할 수 있다.
  - formAction은 action보다 우선 적용된다.

```
export default function Form() {
  async function handleSubmit() {
    'use server';
    // ...
  }

  async function submitImage() {
    'use server';
    // ...
  }

  return (
    <form action={handleSubmit}>
      <input type="text" name="name" />
      <input type="image" formAction={submitImage} />
      <button type="submit">Submit</button>
    </form>
  );
}
```

- StartTransition을 사용한 지정 호출
  - action 또는 formAction없이 Server Action을 사용할 수 있다. useTransition hook에서 startTransition을 사용하여 가능하다.

```
'use client';

import { useTransition } from 'react';
import { addItem } from '../actions';

function ExampleClientComponent({ id }) {
  let [isPending, startTransition] = useTransition();

  return (
    <button onClick={() => startTransition(() => addItem(id))}>
      Add To Cart
    </button>
  );
}
```

```
# app/actions.js
'use server';

export async function addItem(id) {
  await addItemToDb(id);
  revalidatePath(`/product/${id}`);
}
```

- startTranstion없이 사용자 지정 호출
  - mutation없다면 다른 함수와 같이 함수를 직접 전달가능

```
# app/posts/[id]/page.tsx
import kv from '@vercel/kv';
import LikeButton from './like-button';

export default function Page({ params }: { params: { id: string } }) {
  async function increment() {
    'use server';
    await kv.incr(`post:id:${params.id}`);
  }

  return <LikeButton increment={increment} />;
}
```

```
# app/post/[id]/like-button.tsx
'use client';

export default function LikeButton({
  increment,
}: {
  increment: () => Promise<void>;
}) {
  return (
    <button
      onClick={async () => {
        await increment();
      }}
    >
      Like
    </button>
  );
}
```

### 개선사항

- useOptimistic 실험
  - 앱의 반응성을 높여 사용자 경험을 향상 시키는 기술
  - 서버 액션 호출시 서버 액셩 응답을 기다리지 않고 예상 결과를 반영하도록 UI 즉시 업데이트

```
'use client';

import { experimental_useOptimistic as useOptimistic } from 'react';
import { send } from './actions.js';

export function Thread({ messages }) {
  const [optimisticMessages, addOptimisticMessage] = useOptimistic(
    messages,
    (state, newMessage) => [...state, { message: newMessage, sending: true }],
  );
  const formRef = useRef();

  return (
    <div>
      {optimisticMessages.map((m) => (
        <div>
          {m.message}
          {m.sending ? 'Sending...' : ''}
        </div>
      ))}
      <form
        action={async (formData) => {
          const message = formData.get('message');
          formRef.current.reset();
          addOptimisticMessage(message);
          await send(message);
        }}
        ref={formRef}
      >
        <input type="text" name="message" />
      </form>
    </div>
  );
}
```

- useFormStatus 실험
  - useFormStatus hook은 Form Action없이 사용가능하고 이전 pending을 보호한다.

```
import { experimental_useFormStatus as useFormStatus } from 'react-dom';

function Submit() {
  const { pending } = useFormStatus();

  return (
    <input
      type="submit"
      className={pending ? 'button-pending' : 'button-normal'}
      disabled={pending}
    >
      Submit
    </input>
  );
}
```

- 점진적 향상
  - `<form>`이 JS 없이 비활성화 상태에서 제대로 동작 실패한 경우도 데이터 제출 가능

```
'use client';

import { useState } from 'react';
import { handleSubmit } from './actions.js';

export default function ExampleClientComponent({ myAction }) {
  const [input, setInput] = useState();

  return (
    <form action={handleSubmit} onChange={(e) => setInput(e.target.value)}>
      {/* ... */}
    </form>
  );
}
```
