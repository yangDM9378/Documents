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

[사진]

- 경로 생성 후 page 파일에 UI를 작성한다.

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
