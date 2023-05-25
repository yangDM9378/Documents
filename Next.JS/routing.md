# Routing Fundamentals

- v13부터 App Router로 이루어진다.
- 레이아웃, 로딩, 에러 등이 제공되어진다.
- 기존의 Page Router도 사용가능하다.

## App Router

- 폴더는 path를 나타낸다.
- 마지막 폴더에는 반드시 page.js 파일이 필요하다.

## File Conventions

- page.js
- layout.js
- loading.js
- error.js
- global-error.js
- not-found.js

# Defining Routes

- 경로 생성 후 page 파일에 UI를 작성한다.
  <img src='./img/DefiningRoutes.png' width='300'>

# Pages & Layout

## Pages

- 고유한 UI이다.
- page.js로 구성요소를 내보내어 페이지 정의할 수 있다.

## Layout

- 공유하는 UI이다.
- re-render되지 않는다
- Metadata를 활용하여 <head>의 데이터를 바꿀 수 있다.

# Linking and Navigating

- server-centric routing과 client-side navigation을 같이 사용한다.
- <Link> Component
- useRouter() Hook

## Link

```
import Link from 'next/link';

export default function Page() {
  return <Link href="/dashboard">Dashboard</Link>;
}
```

### Props

- href
  - 변수 보내는 작업 가능
  ```
  <Link
  href={{
    pathname: '/about',
    query: { name: 'test' },
  }}
  >
  ```
- replace

  - 현재 히스토리 상태를 새 URL에 보내지 않음

  ```<Link href="/dashboard" replace>
      Dashboard
    </Link>

  ```

- prefetch
  - client에서 탐색 성능 개선에 사용
  ```
      <Link href="/dashboard" prefetch={false}>
      Dashboard
    </Link>
  ```

## useRouter()

- next/navigation에서 import 해서 사용
- ClientComponent에서 사용

```
'use client';

import { useRouter } from 'next/navigation';

export default function Page() {
  const router = useRouter();

  return (
    <button type="button" onClick={() => router.push('/dashboard')}>
      Dashboard
    </button>
  );
}
```

### useRouter 사용

- router.push(href: string)
- router.replace(href: string)
- router.refresh()
- router.prefetch(href: string)
- router.back()
- router.forward()

### Router Events

- pathname은 usePathname()에서 사용
- query는 useSearchParams()에서 사용

```
'use client';


import { usePathname, useSearchParams } from 'next/navigation';

function NavigationEventsImplementation() {
const pathname = usePathname();
const searchParams = useSearchParams();

useEffect(() => {
const url = pathname + searchParams.toString();
// You can now use the current URL
}, [pathname, searchParams]);
}

export function NavigationEvents() {
return (
<Suspense fallback={null}>
<NavigationEventsImplementation />
</Suspense>
);
}
```

# Route Groups

- URL 구조 없이 조직 라우트를 만들 수 있다.
- 특별한 layout route segments opting-in 할 수 있다.
- 멀티 layout을 생성 할 수 있다.

## Convention

- (folderName)으로 생성한다.

## Good to know

- URL 경로에 영향이 가지 않게 특별한 다른 이름을 가져야한다
- 그룹 내부에 같은 URL 경로 설정 불가((a)/about/page.js 와 (b)/about/page.js 같은 경우)
- 그룹 이동시 layout의 전체 페이지 렌더가 일어난다.

# Dynamic Routes

- 미리 랜더링 되어야 할때, 정확한 세그먼트 이름을 알지 못할때 동적 데이터로 경로 생성시 이용한다.

## Convention

- [folderName]으로 생성한다.
- params를 통해 props 된다.

## Example

```
export default function Page({ params }) {
  return <div>My Post</div>;
}
```

## Generating Static Params

- 빌드시 정적으로 경로 생성되게 합니다.

```
export async function generateStaticParams() {
  const posts = await fetch('https://.../posts').then((res) => res.json());

  return posts.map((post) => ({
    slug: post.slug,
  }));
}
```

## Catch-all Segment

- 동적 세그먼트는 순차적인 세그먼트 확장이 가능하다.
- [...folderName]
- app/shop/[...slug]/page.js -> /shop/a/b/c -> { slug: ['a', 'b', 'c'] }

## Optional Catch-all Segments

- optional 하게 parameter를 포함한다.
- [[...folderName]]
- app/shop/[[...slug]]/page.js -> /shop/a/b/c -> { slug: ['a', 'b', 'c'] }

## TypeScript Dynamic Routes

```
export default function Page({ params }: { params: { slug: string } }) {
  return <h1>My Page</h1>;
}
```

# Loading UI and Streaming

- skeleton이나 spinners를 통해 만들어지며 웹에서의 반응으로 사용자 경험 UX를 이끈다.
- component별로 loading창을 띄울 수 있다.

## Convention

- 폴더내의 loading.js 파일 생성

## example

```
import { Suspense } from 'react';
import { PostFeed, Weather } from './Components';

export default function Posts() {
  return (
    <section>
      <Suspense fallback={<p>Loading feed...</p>}>
        <PostFeed />
      </Suspense>
      <Suspense fallback={<p>Loading weather...</p>}>
        <Weather />
      </Suspense>
    </section>
  );
}
```

# Error Handling

- React Error 경계에서 Automatically 라우트 중첩 자식을 자동 매칭한다.
- 특정 오류에 맞는 UI 적용 가능
- 전체 페이지 리로딩 하지않고 복구 시도하는 기능이 가능하다.
- 오류 발생 부분만 오류가 생기고 나머지 기능은 사용가능하다.

## Convention

- 폴더내의 error.js 파일 생성

```
'use client'; // Error components must be Client Components

import { useEffect } from 'react';

export default function Error({
  error,
  reset,
}: {
  error: Error;
  reset: () => void;
}) {
  useEffect(() => {
    // Log the error to an error reporting service
    console.error(error);
  }, [error]);

  return (
    <div>
      <h2>Something went wrong!</h2>
      <button
        onClick={
          // Attempt to recover by trying to re-render the segment
          () => reset()
        }
      >
        Try again
      </button>
    </div>
  );
}
```

## Global-error

```
'use client';

export default function GlobalError({
  error,
  reset,
}: {
  error: Error;
  reset: () => void;
}) {
  return (
    <html>
      <body>
        <h2>Something went wrong!</h2>
        <button onClick={() => reset()}>Try again</button>
      </body>
    </html>
  );
}
```
