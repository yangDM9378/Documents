# Rendering

- 작성한 코드를 유저 인터페이스에 적용하는 것이다

## Rendering Environments

- client와 server로 환경 두개로 렌더링 할 수 있다.

## Component-level Client and Server Rendering

- react18 버전 전에는 클라이언트의 전체를 사용하였다.
- 현재는 클라이언트와 서버 레벨을 지정하여 렌더링 할 수 있다.
- 기본적으로 Server Component로 렌더링 된다

## Static and Dynamic Rendering on the Server

- 최적 렌더링 옵션을 개발자에게 주어진다.

### Static Rendering

- Server와 Client 컴포넌트들은 build time 에서 prerender 할 수 있다.
- 후속 요구에 대해 캐시된다.
- Server와 Client 컴포넌트들은 Static Rendering 되는 동안 다르게 나타난다.
- Client Components는 서버에서 HTML과 JSON을 prerender와 cached 한다.
- cached 결과 hydration 된다.
- Server Components는 리엑트로부터 payload는 HTML 생성에 사용한다.

### Dynamic Rendering

- Server와 Client 컴포넌트는 서버로 요청시 렌더된다.
- cached 되지 않는다

## Edge and Node.js Runtimes

- 서버에서 페이지를 렌더시 두 Runtime이 있다.
- Node.js runtime은 기본값이고 Node.js API와 호환가능한 페기지 접근시 나타난다.
- Edge Runtime은 Web APIs의 기본이다.

# Static and Dynamic Rendering

- static route는 서버에서 빌드시 나타난다.
- dynamic route는 서버에 요구시 나타난다.

## Static Rendering

- 기본값으로 포퍼먼스를 향상시킨다.
- CDN에서 적용된다.

## Static Data Fetching

- 기본값으로 데이터를 fetch()할때 cache 된다.
- force-cache option을 통해 cache하지 않을 수 잇다.

## Dynamic Rendering

- static rendering 하는 동안, dynamic fetch()요구는 dynamic하게 설정가능하다.

### Dynamic Functions

- Dynamic Function은 유저쿠키, 현재 요구 해더, URL's serch param의 정보에 의존한다.
- Server 컴포넌트에서 cookies()와 headers() 사용은 전체 route가 dynamic 랜더링으로 선택된다.
- Client 컴포넌트에서 useSearchParams()는 static 렌더링과 모든 클라이언트 컴포넌트 랜더 대신에 생략할 것이다.
  - useSearchParams()를 사용시 <Suspense/>를 사용을 추천한다. <Suspense/>에 있는 클라이언트 컴포넌트를 statically 랜더 하게 할 것이다.

```
import { Suspense } from 'react';
import SearchBar from './search-bar';

// This component passed as a fallback to the Suspense boundary
// will be rendered in place of the search bar in the initial HTML.
// When the value is available during React hydration the fallback
// will be replaced with the `<SearchBar>` component.
function SearchBarFallback() {
  return <>placeholder</>;
}

export default function Page() {
  return (
    <>
      <nav>
        <Suspense fallback={<SearchBarFallback />}>
          <SearchBar />
        </Suspense>
      </nav>
      <h1>Dashboard</h1>
    </>
  );
}
```

## Dynamic Data Fetching

- fetch() 요구를 cache하지 않기 위해 옵션을 no-store 또는 revalidte를 0으로 지정한다.

# Edge and Node.js Runtimes

- next.js의 runtimes은 실행가능 라이브러리, APIs, 기타 기능 집합을 의미한다.
- node.js Runtime/Edge Runtime이 있다.
- 기본값은 Node.js runtime이다. 다른 런타임도 지정 가능하다.

## Runtime Differences

### Edge Runtime

- 경량 Edge Runtime은 node.js APIs를 사용가능하다.
- 낮은 지역시간으로 작고, 심플한 기능을 할 수 있다.

### Node.js Runtime

- node.js APIs 접근을 사용할 수 있지만 빠르지 않다.

### Serverless node.js

- Edge Runtime보다 더 복잡한 부하를 처리하기 위해서 확장 가능 솔루션으로 서버리스가 있다.

### 예시

```
export const runtime = 'edge'; // 'nodejs' (default) | 'edge'
```
