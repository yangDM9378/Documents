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
  dangerouslySetInnerHTML={{
    __html: `document.getElementById('banner').classList.remove('hidden')`,
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
