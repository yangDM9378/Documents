# Optimiztions

- 애플리케이션속도와 core web vitals를 향상시키는 다양한 방법이있다.
- 사용자 경험을 증가 시킬수 있다.

## Built-in Components

- UI 최적화 향상의 복잡성을 추상화한다.
- Images: img 태그요소를 기반으로 지연 로딩 및 자동으로 디바이스 사이즈에 따라 이미지 조정 하는 데 이미지 최적화한다.
- Link: a 태그요소를 기반으로 background에서 더 빠르고 부드러운 페이지 전환을 위해 prefetch한다.
- Script: script 태그를 기반으로 다른 스크립트의 로드 및 실행을 제어한다.

## Metadata

- Metadata는 SEO에 내용을 더 이해하게 하고 소셜 미디어에 표시되는 방식을 사용자 정의 가능하게 한다.
- Metadata API는 페이지의 head 태그를 수정하여 사용한다.
- 2가지 방법으로 metadata를 configure 할 수 있다.

1. config-based metadata: dynamic generateMetadata함수를 layout.js 또는 page.js 파일에 static 형태로 export 한다.
2. File-based Metadata: 라우터 세그먼트에 static 또는 dynamic 파일을 추가한다.

- imageResponse 생성자 함께 JSX와 CSS에 사용하여 동적 오픈 Graph Images를 사용가능하다.

## Static Assets

- /public 폴더는 이미지나 폰트 그리고 다른 파일들을 server static assets처럼 사용할 수 있다.
- /public 내부 파일은 CDN 공급자가 캐시할 수 있다.

## analtics and Monitoring

- 큰 어플리케이션을 위해 인기있는 분석과 모니터링 툴을 통합되어 어플리케이션 성능 이해에 도움 된다.

# Image Optimization

- <img> 요소의 확장이다.
- Size Optimization: 각 이미지를 위한 자동 서버 맞춤 사이즈 맞춤은 WebP와 AVIF 이미지 형식으로 사용한다.
- Visual Stablity: 이미지의 로딩동안 자동으로 레이아웃 이동 방지한다.
- Faster Page Loads: 선택적 블러 처리를 통해 기본 브라우저 지연 로딩을 사용하여 뷰포트에 들어갈 때만 로드 되게 한다.
- Asset Flexibility: 원격 서버에 저장된 이미지에 대해서도 ondemend 이미지 크기 조정한다.

## 사용법

```
import Image from 'next/image';
```

### Local Images

- lcoal image 사용을 위해 .jpg, .png, .webp 이미지 파일을 import한다.
- 가져온 파일 기반으로 이미지의 너비와 높이를 자동으로 결정한다. 이미지 로드시 누적 레이아웃 이동을 방지한다.

```
import Image from 'next/image';
import profilePic from './me.png';

export default function Page() {
  return (
    <Image
      src={profilePic}
      alt="Picture of the author"
      // width={500} automatically provided
      // height={500} automatically provided
      // blurDataURL="data:..." automatically provided
      // placeholder="blur" // Optional blur-up while loading
    />
  );
}
```

### Remote Images

- URL string으로 scr에 사용할 수 있다.
- 빌드 프로세스 중 원격 파일에 엑세스 할 수 없으므로 너비, 높이 및 선택 blurDataURL 소품을 수동으로 제공해야한다.

```
import Image from 'next/image';

export default function Page() {
  return (
    <Image
      src="https://s3.amazonaws.com/my-bucket/profile.png"
      alt="Picture of the author"
      width={500}
      height={500}
    />
  );
}
```

- 안전하게 이미지 최적화를 위해 URL patterns을 next.config.js에 제공해야한다.

```
module.exports = {
  images: {
    remotePatterns: [
      {
        protocol: 'https',
        hostname: 's3.amazonaws.com',
        port: '',
        pathname: '/my-bucket/**',
      },
    ],
  },
};
```

### Domains

- 원격 이미지 최적화를 원하고 하는데 next.js 이미지 최적화 API를 사용할 수 있다.
- 로더를 기본 설정에 두고 절대 URL을 입력할 수 있다.

### Loaders

- 이미지를 위한 URLs 생성 함수이다.
- src에 제공하여 수정하고 다중 생성 URL들은 다른 사이즈의 이미지를 요구한다.
- 다중 URL들은 자동 srcset 생성에 사용되므로 사이트 방문자에게 뷰포트에 적합한 크기의 이미지가 제공된다.
- 모든 위치에서 이미지 최적화를 한 다음 웹 서버에서 직접 제공하는 내장 이미지 최적화 API를 사용합낟.
- CDN 또는 이미지 서버에서 직접 이미지를 제공하는 경우 JS로 고유한 로더 함수를 작성할 수 있다.

### Priority

- 각 페이지에 큰 요소가 될 이미지의 우선순위 속성을 추가하여 로드 이미지 순서를 지정할 수 있다.
- LPC 요소는 일만적으로 페이지 뷰포트내에서 볼 수 있는 가장 큰 이미지 텍스트 블록이다.
- LPC 요소가 priority속성이 없는 경우 콘솔 경고가 발생한다.

```
import Image from 'next/image';
import profilePic from '../public/me.png';

export default function Page() {
  return <Image src={profilePic} alt="Picture of the author" priority />;
}
```

## Image Sizing

- 이미지 성능 저해는 이미지가 로드될 때 페이지의 다른 요소를 밀어 내는 레이아웃 이동을 통한 것이다.
- 이 문제는 Cumulative Layout shift라고 불리는 Core Web Vital이다.
- 해당 문제를 피하기 위해 항상 이미지 크기를 조정하는 것이 있다.
- 브라우저는 이미지가 로드 전 이미지 공간을 미리 생성하여 방지 할 수 있다.
- next/image는 3가지 방법으로 좋은 포퍼먼스를 이끈다.

1. 자동으로 static import 가져오기
2. 명시적으로 width와 height 적기
3. 암묵적으로, 이미지 확장되어 부모 요소를 채우는 채우기 사용한다.

## Styling

- 이미지 컴포넌트의 스타일링은 보통 <img>요소 스타일 지정과 비슷하지만 유의해야 할 몇 가지 지침이 있다.
- className 또는 style사용/styled-jsx 사용 금지
- fill 사용시 부모 요소를 position: relative 해야함
- fill 사용시 자식 요소를 display: block 해야함

# Font Optimization

- next/font는 자동으로 최적화 할것이고 개인정보 보호 및 성능향상을 위해 외부 네트워크 요청을 제거합니다.
- font file을 위해 build-in 자동 self-hosting을 포함한다.
- 사용된 기본 CSS size-adjust 속성 덕에 레이아웃 이동 없이 웹 글꼴 최적 로드 할 수 있다.
- Google Font를 편리하게 사용할 수 있게 해준다
- CSS와 font file들은 빌드시 다운로드 되며 나머지 static 파일과 함께 자체 호스팅 된다.
- Google 브라우저에게 요청을 보내지 않아도 된다.

## Google Fonts

- 자동으로 Google Font의 self-host한다.
- 글꼴은 배포에 포함되며 배포자와 동일한 도메인에서 제공된다.
- next/font/goolge 함수에서 사용할 수 있다.
- 가장 좋은 성능과 편리함을 위해 다양한 폰트를 추천한다.

```
# app/layout.tsx
import { Inter } from 'next/font/google';

// If loading a variable font, you don't need to specify the font weight
const inter = Inter({
  subsets: ['latin'],
  display: 'swap',
});

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en" className={inter.className}>
      <body>{children}</body>
    </html>
  );
}
```

- 가변 글꼴 사용할 수 없는 경우, 지정해 주어야한다.

```
# app/layout.tsx
import { Roboto } from 'next/font/google';

const roboto = Roboto({
  weight: '400',
  subsets: ['latin'],
  display: 'swap',
});

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en" className={roboto.className}>
      <body>{children}</body>
    </html>
  );
}
```

- 다양한 굵기도 조정 가능하다

```
# app/layout.tsx
const roboto = Roboto({
  weight: ['400', '700'],
  style: ['normal', 'italic'],
  subsets: ['latin'],
  display: 'swap',
});
```

### 하위 집합 지정

- 구글 폰트는 자동으로 하위 집합 지정한다.
- 이것은 font file 크기가 줄고 향상된 포퍼먼스를 가진다.
- preload를 원한다면 subset을 정이할 필요가 있다.
- preload가 참일 때 하위 집합을 지정하지 않으면 경고 표시된다.

```
const inter = Inter({ subsets: ['latin'] });
```

### 다양한 폰트 사용

- 첫접근은 utility 함수를 만들어 className에 폰트를 사용하는 것이다.
- 글꼴 로드시 마다 미리 렌더 된다.

```
# app/fonts.ts
import { Inter, Roboto_Mono } from 'next/font/google';

export const inter = Inter({
  subsets: ['latin'],
  display: 'swap',
});

export const roboto_mono = Roboto_Mono({
  subsets: ['latin'],
  display: 'swap',
});
```

```
import { inter } from './fonts';

export default function Layout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en" className={inter.className}>
      <body>
        <div>{children}</div>
      </body>
    </html>
  );
}
```

```
import { roboto_mono } from './fonts';

export default function Page() {
  return (
    <>
      <h1 className={roboto_mono.className}>My page</h1>
    </>
  );
}
```

- 위처럼 사용시 Inter이 글로벌이 되고 Roboto Mono는 import가 필요하게 된다.
- 대체로 CSS 변수를 생성하고 선호하는 CSS 솔루션과 함께 사용 가능하다.

```
import { Inter, Roboto_Mono } from 'next/font/google';
import styles from './global.css';

const inter = Inter({
  subsets: ['latin'],
  variable: '--font-inter',
  display: 'swap',
});

const roboto_mono = Roboto_Mono({
  subsets: ['latin'],
  variable: '--font-roboto-mono',
  display: 'swap',
});

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en" className={`${inter.variable} ${roboto_mono.variable}`}>
      <body>
        <h1>My App</h1>
        <div>{children}</div>
      </body>
    </html>
  );
}
```

```
html {
  font-family: var(--font-inter);
}

h1 {
  font-family: var(--font-roboto-mono);
}
```

## Local Fonts

- next/font/local과 src를 통해 local font file을 사용한다.

```
import localFont from 'next/font/local';

// Font files can be colocated inside of `app`
const myFont = localFont({
  src: './my-font.woff2',
  display: 'swap',
});

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en" className={myFont.className}>
      <body>{children}</body>
    </html>
  );
}
```

- multiple 파일 사용하기

```
const roboto = localFont({
  src: [
    {
      path: './Roboto-Regular.woff2',
      weight: '400',
      style: 'normal',
    },
    {
      path: './Roboto-Italic.woff2',
      weight: '400',
      style: 'italic',
    },
    {
      path: './Roboto-Bold.woff2',
      weight: '700',
      style: 'normal',
    },
    {
      path: './Roboto-BoldItalic.woff2',
      weight: '700',
      style: 'italic',
    },
  ],
});
```

## Tailwind CSS와 함께 하기

- next/font는 tailwind CSS에서 사용할 수 있다.

```
# app/layout.tsx
import { Inter, Roboto_Mono } from 'next/font/google';

const inter = Inter({
  subsets: ['latin'],
  display: 'swap',
  variable: '--font-inter',
});

const roboto_mono = Roboto_Mono({
  subsets: ['latin'],
  display: 'swap',
  variable: '--font-roboto-mono',
});

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en" className={`${inter.variable} ${roboto_mono.variable}`}>
      <body>{children}</body>
    </html>
  );
}
```

- tailwind CSS config 파일

```
# tailwind.config.js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    './pages/**/*.{js,ts,jsx,tsx}',
    './components/**/*.{js,ts,jsx,tsx}',
  ],
  theme: {
    extend: {
      fontFamily: {
        sans: ['var(--font-inter)'],
        mono: ['var(--font-roboto-mono)'],
      },
    },
  },
  plugins: [],
};
```

## Preloading

- 사이트 페이지에서 글꼴 기능이 호출 시 전역적으로 사용할 수 없으며 모든 경로에 미리 로드된다.
- 글꼴은 사용되는 파일 유형에 따라 경로에서만 사전 로드 된다.

1. 고유페이지인 경우 해당 페이지의 고유 경로에 preloading된다.
2. 레이아웃인 경우 레이아웃에 의해 래핑된 모든 경로에 미리 로드된다.
3. 루트 레이아웃인 경우 모든 경로에 미리 로드된다.

## 글꼴 재사용

- 하나의 호스팅으로 호출된다.
- 다중 파일에 같은 font 함수를 로드할 경우 다중 인스턴스로 부터 같은 폰트가 호스팅된다.

1. 하나의 공유 파일에서 폰트 로더 기능 호출하기
2. 상수로 내보내기
3. 각 파일에서 상수 가져오기

# Script Optimization

## Layout Scripts

- 다중 라우터를 위한 third-party script 로드를 위해서 next/script를 가져와 레이아웃 구성 요소에 직접 스크립트를 포함한다.

```
# app/dashboard/layout.tsx
import Script from 'next/script';

export default function DashboardLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <>
      <section>{children}</section>
      <Script src="https://example.com/script.js" />
    </>
  );
}
```

- 사용자가 폴더 경로 또는 중첩 경로에 엑세스하면 타사 스크립트를 가져온다.

### Application Scripts

- 모든 라우트를 위해 타사 스크립트 로드는 next/script를 가져오거나 직접 root script에 포함한다.
- 스크립트는 애플리케이션 경로에 엑세스시 로드되고 시행된다.
- 여러 페이지를 탐색하더라도 스크립트가 한 번만 로드 되도록 한다.

### 전략

- next/script의 기본 행동을 통해 타사 스크립트 페이지 또는 레이아웃에서 로드할수 있지만 strategy을 통해 조절 할 수 있다.

1. beforeInteractive: 코드이전과 페이지 hydration이 발생하기 전에 스크립트를 로드한다.
2. afterInteractive: (default) 초기에 스크립트를 로드하지만 페이지에 약간의 hydration이 발생 후에 로드한다.
3. lazyOnload: 브라우저 유휴 시간 동안 스크립트 로드한다
4. worker: web worker에서 스크립트를 로드한다.

### 웹 작업자한테 스크립트 offloading

- partytown과 함께 웹 작업자에서 오프로드 및 실행된다.
- main 쓰레드의 헌신으로 사이트 성능을 향상 할 수 있다.

```
module.exports = {
  experimental: {
    nextScriptWorkers: true,
  },
};
```

```
npm install @builder.io/partytown
```

- 설치 완료시 stategy='worker'은 어플리 케이션에서 자동으로 Partytown을 인스턴스화하고 웹 작업자에게 스크립트를 오프로드한다.

```
import Script from 'next/script';

export default function Home() {
  return (
    <>
      <Script src="https://example.com/script.js" strategy="worker" />
    </>
  );
}
```

### Inline Scripts

- 인라인 스크립트 또는 스크립트는 외부 파일로 부터 로드하지 않은 스크립트도 스크립트 구성요소에서 지원된다.

```
<Script id="show-banner">
  {`document.getElementById('banner').classList.remove('hidden')`}
</Script>
```

- dangerouslySetInnerHTMl 속성으로 사용할 수 있다.

```
<Script
  id="show-banner"
  dangerouslySetInnerHTML={{    __html: `document.getElementById('banner').classList.remove('hidden')`,
  }}
/>
```

### 추가 코드 실행

- 특정 이벤트가 발생한 후 추가 코드를 실행하기 위해 스크립트 구성 요소와 함께 이벤트 핸들러를 사용할 수 있다.

1. onLoad
2. onReady
3. onError

```
'use client';

import Script from 'next/script';

export default function Page() {
  return (
    <>
      <Script
        src="https://example.com/script.js"
        onLoad={() => {
          console.log('Script has loaded');
        }}
      />
    </>
  );
}
```

### 추가 속성

- nonce 또는 사용자 지정 데이터 속성과 같은 Script구성 요소에 사용않는 script 요소에 할당 가능한 DOM 속성이 있다.

```
import Script from 'next/script';

export default function Page() {
  return (
    <>
      <Script
        src="https://example.com/script.js"
        id="example-script"
        nonce="XUENAJFW"
        data-test="script"
      />
    </>
  );
}
```

# Metadata

- 향상된 SEO 및 웹 공유 가능성을 위해 metadata(HTML head element의 meta와 link 태그)를 정의하는 데 사용할 수 있는 메타데이터 API가 있다.
- 사용방법

1. Config-based Metadata: layout.js 또는 page.js 파일에 static metadata object 또는 dynamic generateMetadata 함수를 내보낸다
2. File-based Metadata: route 세그먼트에 static 또는 dynamically로 생성된 파일을 추가한다.

- 두가지 옵션을 모두 사용시 페이지에 대한 head 요소를 자동으로 생성 ImageResponse 생성자를 사용하여 동적 이미지 생성이 가능하다.

## Static Metadata

- static metadata를 정의하기 위해 layout.js 또는 static page.js 파일을 Metadata object를 내보낸다.

```
# layout.tsx/page.tsx

import { Metadata } from 'next';

export const metadata: Metadata = {
  title: '...',
  description: '...',
};

export default function Page() {}
```

## Dynamic Metadata

- generateMetadata 함수를 사용하여 동적인 값이 필요한 메타데이터를 가져온다.

```
# app/products/[id]/page.tsx

import { Metadata, ResolvingMetadata } from 'next';

type Props = {
  params: { id: string };
  searchParams: { [key: string]: string | string[] | undefined };
};

export async function generateMetadata(
  { params, searchParams }: Props,
  parent?: ResolvingMetadata,
): Promise<Metadata> {
  // read route params
  const id = params.id;

  // fetch data
  const product = await fetch(`https://.../${id}`).then((res) => res.json());

  // optionally access and extend (rather than replace) parent metadata
  const previousImages = (await parent).openGraph?.images || [];

  return {
    title: product.title,
    openGraph: {
      images: ['/some-specific-page-image.jpg', ...previousImages],
    },
  };
}

export default function Page({ params, searchParams }: Props) {}
```

- generateMetadata를 통한 정적 및 동적 메타데이터는 모두 서버 구성 요소에만 지원된다.
- route를 렌더할때 Next.js는 generateMetadata, generateStaticParams, Layouts, Pages 및 Server Componenets에서 동일한 데이터에 대한 fetch 요청을 자동으로 중복 제거한다. fetch를 사용할 수 없는 경우 React 캐시를 사용하여 가져온다.

## File-based metadata

- 특수 파일은 metadata 사용가능

1. favicon.ico, apple-icon.jpg, and icon.jpg
2. opengraph-image.jpg and twitter-image.jpg
3. robots.txt
4. sitemap.xml

- 정적 metadata에 대해 사용하거나 코드를 사용하여 프로그래밍방식으로 파일을 생성 할 수 있다.
- 구현 및 예제는

## Behavior

- 파일베이스 metadata는 우선 순위가 높으면 config-based metadata를 오버라이딩한다.

### 기본 필드

- 두개의 기본 meta 태그들은 항상 정의하지 않는 경우 두가지 기본 meta tag가 있다.

1.  meta charset tag는 웹사이트의 문자 인코딩을 설정한다.
2.  meta viewport tag는 웹사이트의 뷰포트 너비와 배율을 설정하여 다양한 장치에 맞게 조정한다

```
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
```

### Ordering

- metadata는 루트 세그먼트에서 시작하여 page.js 세그먼트에 가까운 세그먼트까지 순서대로 평가된다.

1. app/layout.tsx
2. app/blog/layout.tsx
3. app/blog/[slug]/page.tsx

### Merging

- 멀티 세그먼트로부터 같은 라우터에서 metadata object는 최종 metadata 출력을 구성하여 나타난다.
- 중복 키는 순서에 따라 대체된다.
- 세그먼트에서 정의된 openGraph 및 robot과 같은 중첩 필드시 마지막 세그먼트로 덮어쓴다.

```
# app/layout.js

export const metadata = {
  title: 'Acme',
  openGraph: {
    title: 'Acme',
    description: 'Acme is a...',
  },
};
```

```
# app/blog/page.js

export const metadata = {
  title: 'Blog',
  openGraph: {
    title: 'Blog',
  },
};

// Output:
// <title>Blog</title>
// <meta property="og:title" content="Blog" />
```

- 위의 예제에서

1. app/layout.js로부터 title은 app/blog/page.js에서 title로 교체된다.
2. app/blog/page.js가 openGraph metadata를 설정하기 때문에 app/layout.js의 모든 openGraph 필드는 app/blog/page.js에서 대체된다.
3. openGraph.description이 없다는 점에 유의한다.

- 만일 세그먼트 간에 일부 중첩 필드를 공유하면서 다른 필드로 덮어쓰려는 경우 별도 변수를 가져올 수 있다.

```
# app/shared-metadata.js

export const openGraphImage = { images: ['http://...'] };

```

```
# app/page.js

import { openGraphImage } from './shared-metadata';

export const metadata = {
  openGraph: {
    ...openGraphImage,
    title: 'Home',
  },
};
```

```
# app/about/page.js

import { openGraphImage } from '../shared-metadata';

export const metadata = {
  openGraph: {
    ...openGraphImage,
    title: 'About',
  },
};
```

- 위의 예제에서

1. OG image는 app/layout.js와 app/about/page.js 사이에 다른 타이들 간에 공유 된다.
   </br>
   **Inheriting fileds**

```
# app/layout.js

export const metadata = {
  title: 'Acme',
  openGraph: {
    title: 'Acme',
    description: 'Acme is a...',
  },
};
```

```
# app/about/page.js

export const metadata = {
  title: 'About',
};

// Output:
// <title>About</title>
// <meta property="og:title" content="Acme" />
// <meta property="og:description" content="Acme is a..." />
```

- 위의 예제에서

1. app/layout.js로부터 title은 app/about/page.js의 타이틀로 교체된다.

## Dynamic Image Generation

- ImageResponse 생성자는 JSX와 CSS에서 사용할 dynamic image를 생성하기를 허용한다.
- Open Graph 이미지, Twitter 카드 등과 같은 소셜 미디어 이미지를 만드는데 유용하다.
- ImageResponse는 EdgeRuntime을 사용하고, Next.js는 edge의 캐시된 image로부터 자동으로 맞는 header을 추가하고 성능과 재계산을 줄인다.
- next/server로부터 ImageResponse를 import하여 사용한다.

```
# app/about/route.jsx

import { ImageResponse } from 'next/server';

export const runtime = 'edge';

export async function GET() {
  return new ImageResponse(
    (
      <div
        style={{
          fontSize: 128,
          background: 'white',
          width: '100%',
          height: '100%',
          display: 'flex',
          textAlign: 'center',
          alignItems: 'center',
          justifyContent: 'center',
        }}
      >
        Hello world!
      </div>
    ),
    {
      width: 1200,
      height: 600,
    },
  );
}
```

- ImageResponse는 route handler 및 파일기반 metadata를 포함하여 next.js API와 잘 통합된다.
- 예를들어, opengraph-image.tsx 파일에 ImageResponse를 사용하여 빌드 시 또는 요청 시 동적으로 오픈 그래프 이미지를 생성한다.
- ImageResponse는 flexbox와 absolute positioning CSS, custon fonts, text wrapping, nest images를 포함한다.
- edge rungime만 지원된다.
- 그리드와 같은 고급 레이아웃은 지원되지 않는다.
- 500KB 최대 번들을 지원하며, JSX, CSS, 글꼴, 이미지 및 기타 하위속성이 포함된다.
- ttf, otf 및 woff 글꼴 형식만 지원된다.(분석 속도를 최대화 하려면 ttf 또는 otf를 사용하는 것이 좋다)

## JSON-LD

- 콘텐츠를 이해하기 위해 검색 엔진에서 사용할 수 있는 구조화된 데이터 형식이다.
- layout.js 또는 page.js 구성요소에서 script 태그로 렌더링 하는 것이 권장사항이다.

```
# app/products/[id]/page.tsx

export default async function Page({ params }) {
  const product = await getProduct(params.id);

  const jsonLd = {
    '@context': 'https://schema.org',
    '@type': 'Product',
    name: product.name,
    image: product.image,
    description: product.description,
  };

  return (
    <section>
      {/* Add JSON-LD to your page */}
      <script
        type="application/ld+json"
        dangerouslySetInnerHTML={{ __html: JSON.stringify(jsonLd) }}
      />
      {/* ... */}
    </section>
  );
}
```

- 구글용 리치 결과 테스트 또는 일반 스키마 마크업 검사기로 구조화된 데이터의 유효성을 검사하고 테스트할 수 있다.
- schema-dts를 통해 TypeScript로 JSON-LD를 입력할 수 있다.

```
import { Product, WithContext } from 'schema-dts';

const jsonLd: WithContext<Product> = {
  '@context': 'https://schema.org',
  '@type': 'Product',
  name: 'Next.js Sticker',
  image: 'https://nextjs.org/imgs/sticker.png',
  description: 'Dynamic at the speed of static.',
};
```

# Static Assets

- 루트 디렉토리 public에 static files, image를 제공할 수 잇다.
- 기본 URL에서 시작하는 코드에서 public 내부 파일을 참조 가능하다.

```
import Image from 'next/image';

function Avatar() {
  return <Image src="/me.png" alt="me" width="64" height="64" />;
}

export default Avatar;
```

- robots.txt, favicon.ico, Google 사이트 확인 및 기타 정적 파일에 유용하다.

# Lazy Loading

- Next.js에서 Lazy Loading은 route를 렌더링하는데 필요한 JS의 양을 줄임으로써 어플리케이션의 초기 로딩 성능 개선에 도움이 된다.
- 이를 통해 클라이언트 컴포넌트 및 라이브러리의 로드를 연기하고 필요할 때만 클라이언트 번들에 포함할 수 있다.
- 예를 들어, 사용자가 모달을 클릭하여 열 때 모달 로드를 연기할 수 있다.
- Next.js에서 향상된 lazy loading 2가지 방법이있다.

1. Dynamic next/dynamic을 import하여 사용한다.
2. Suspense와 React.lazy()를 사용한다.

- 기본적으로 서버 구성 컴포넌트는 자동으로 코드 분할되며 스트리밍을 사용하여 서버에서 클라이언트로 UI 조각을 보낼 수 있다. lazy loading은 클라이언트 컴포넌트에 적용된다.

## next/dynamic

- next/dynamic은 React.lazy()와 Suspense가 합쳐진 것이다.
- incremental migration을 허용하기 위해 앱 및 페이지 폴더에서 동일한 방식으로 작동한다.

## 예시

### Importing Client Components

```
# app/page.js

'use client'

import { useState } from 'react'
import dynamic from 'next/dynamic'

// Client Components:
const ComponentA = dynamic(() => import('../components/A'))
const ComponentB = dynamic(() => import('../components/B'))
const ComponentC = dynamic(() => import('../components/C'), { ssr: false })

export default function ClientComponentExample() {
  const [showMore, setShowMore] = useState(false)

  return (
    <div>
      {/* Load immediately, but in a separate client bundle */}
      <ComponentA />

      {/* Load on demand, only when/if the condition is met */}
      {showMore && <ComponentB />}
      <button onClick={() => setShowMore(!showMore)}>Toggle</button>

      {/* Load only on the client side */}
      <ComponentC />
    </div>
  )
}
```

### Skipping SSR

- React.lazy()와 Suspense 사용할때, Client 컴포넌트들은 기본으로 SSR로 pre-render된다.
- Client 컴포넌트를 위해 pre-rendering 하지 않는 다면, ssr 옵션을 false로 변경하면 된다.

```
const ComponentC = dynamic(() => import('../components/C'), { ssr: false })
```

### Importing Server Components

- dynamic하게 Server Components를 가져오면 서버 컴포넌트 자체가 아니라 서버 컴포넌트의 하위인 클라이언트 구성 요소만 lazy loading 된다.

```
# app/page.js

import dynamic from 'next/dynamic'

// Server Component:
const ServerComponent = dynamic(() => import('../components/ServerComponent'))

export default function ServerComponentExample() {
  return (
    <div>
      <ServerComponent />
    </div>
  )
}
```

### Loading External Libraries

- import() 함수를 사용하여 필요에 따라 외부 라이브러리를 로드 가능하다.
- 이 것은 fuzzy search(검색 알고리즘을 사용하여 패턴과 대략적 일치 문자열 찾는 기술)을 위해 fuse.js라이브러리로 확장사용한 예시이다.
- 모듈은 사용자가 검색 입력을 입력한 후 클라이언트에 로드된다.

```
# app/page.js

'use client'

import { useState } from 'react'

const names = ['Tim', 'Joe', 'Bel', 'Lee']

export default function Page() {
  const [results, setResults] = useState()

  return (
    <div>
      <input
        type="text"
        placeholder="Search"
        onChange={async (e) => {
          const { value } = e.currentTarget
          // Dynamically load fuse.js
          const Fuse = (await import('fuse.js')).default
          const fuse = new Fuse(names)

          setResults(fuse.search(value))
        }}
      />
      <pre>Results: {JSON.stringify(results, null, 2)}</pre>
    </div>
  )
}
```

### Adding a custom loading component

```
# app/page.js

import dynamic from 'next/dynamic'

const WithCustomLoading = dynamic(
  () => import('../components/WithCustomLoading'),
  {
    loading: () => <p>Loading...</p>,
  }
)

export default function Page() {
  return (
    <div>
      {/* The loading component will be rendered while  <WithCustomLoading/> is loading */}
      <WithCustomLoading />
    </div>
  )
}
```

### Importing Named Exports

- named된 export를 동적으로 가져오기 위해 import()에 의해 반환된 Promise에서 반환 가능하다.

```
# components/hello.js

'use client'

export function Hello() {
  return <p>Hello!</p>
}
```

```
# app/page.js

import dynamic from 'next/dynamic'

const ClientComponent = dynamic(() =>
  import('../components/hello').then((mod) => mod.Hello)
)
```

# Analytics

- Next.js Speed Insights 사용시 다양한 metrics를 사용하여 페이지 성능 분석 측정이 가능하다.
- vercel 배포에서 zero구성으로 Real Experience Score 수집을 시작할 수 있다.

## Web Vitals

1. Time to First Byte(TTFB): 브라우저가 페이지를 요청하는 시점과 서버에서 정보의 첫 바이트를 수신하는 시점 사이의 시간/HTTPS를 통해 이루어진 경우 DNS 조회 및 TCP handshake와 SSL handshake를 사용하여 연결 설정이 포함
2. First Contentful Paint(FCP): 브라우저가 DOM에서 콘텐츠의 첫 비트를 렌더링하여 페이지가 실제로 로드되고 있다는 첫 피드백을 사용자에게 제공하는 것
3. Largest Contentful Paint(LCP): 페이지의 메인 콘텐츠가 로드되었을 가능성이 있을 때 페이지 로드 타임라인에 해당 시점을 표시하므로 사용자가 감지하는 로드 속도를 측정 할 수 있는 사용자 중심 메트릭입니다.
4. First Input Delay(FID): 사용자가 응답하지 않는 페이지와 상호 작용할 때 느끼는 경험을 수량화 하기 때문에 로드 반응성을 측정하기 위한 사용자 중심 메트릭이다.
5. Cumulative Layout Shift(CLS): 사용자가 예상치 못한 레이아웃 이동을 경험하는 빈도를 수량화 하므로 시각적 안정성을 측정할 때 사용자 중심 메트릭이다.
6. Interaction to Next Paing(INP): 사용자가 페이지에 수행한 모든 상호 작용의 대기 시간을 관찰하고 모든 상호 작용이 있는 단일 값으로 보고된다. INP가 낮으면 상호 작용이 빠르다는 응답을 의미한다.
