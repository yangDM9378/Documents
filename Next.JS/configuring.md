# TypeScript
- Next.js는 React 애플리케이션 구축을 위한 TypeScript 개발 경험을 제공한다
## New Projects
- create-next-app typescript를 기본값으로 설정가능하다
```
npx create-next-app@latest
```
### Existing Projects
- .ts/.tsx 파일로 이름을 지정으로 TypeScript 프로젝트를 추가한다.
- next dev와 next build로 시작시 tsconfig.json 파일을 추가하여 종속성을 지정할 수 있다.
## TypeScript Plugin
- Next.js에는 VSCode 및 코드 편집기에서 type checker 및 자동완성을 사용할 수 있는 플러그인 및 유형 검사기가 포함되어 있다.
- 플러그인 자동 활성화를 놓친경우
1. ctrl+shift+p로 command palette 열기
2. TypeScript: Select TypeScript Version을 검색
3. Use Workspace Version 선택
- 파일을 편집할 때 사용자 플러그인 활성화된다.
- 다음 빌드 실행대 사용자 정의 유형 검사기가 사용된다.
- VSCode 설정 파일을 생성하여 프로세스 자동화 한다.
### Plugin Features
- TypeScript plugin은 
- 세그먼트 구성 옵션에 잘못된 값이 전달시 경고 표시
- 사용 가능 옵션 및 상황에 맞는 문서 표시
- use client 지시문이 올바르게 사용되었는지 확인
- client hook이 클라이언트 구성요소에서만 사용되도록 한다.
## Statically Type Links
- Next.js는 next/link를 사용할 때 오타 및 기타 오류 방지위해 링크를 정적으로 입력 가능
- 페이지 간을 탐색할 때 유형 안전성이 향상
- experimental.typeRoutes를 활성화 하여 typescript를 사용하기
```
# next.config.js

/** @type {import('next').NextConfig} */
const nextConfig = {
  experimental: {
    typedRoutes: true,
  },
}
 
module.exports = nextConfig
```
## 비동기 서버 컴포넌트 typescript dpfj
- typescripy 5.1.3 또는 @types/react 18.2.8보다 높은 버전 사용하기
- 버전 오류시 Promise<Element>' is not a valid JSX element 해당 오류 발생
## 서버 및 클라이언트 구성 요소 간 데이터 전달
- props를 통해 서버와 클라이언트 구성 요소 간에 데이터 전달할 때 데이터는 브라우저에서 사용할 수 있도록 직렬화 한다.
## Path aliases and baseUrl
- tsconfig.json 'paths', 'baseUrl' 옵션으로 자동 지원
## next.config.js 타입 체크
- babel 또는 typescript에 의해 분석 되지 않으므로 JS 파일이어야 하지만 JSDoc을 사용하여 일부 유형 검사 추가 가능
```
// @ts-check
 
/**
 * @type {import('next').NextConfig}
 **/
const nextConfig = {
  /* config options here */
}
 
module.exports = nextConfig
```
# ESLint
- package.json에 스크립트 추가로 즉시 사용가능
- npm run lint 또는 yarn lint
```
# package.json

{
  "scripts": {
    "lint": "next lint"
  }
}
```
- 세가지 옵션 중 하나 선택 가능
1. Strict: 엄격한 Core Web Vitals 규칙과 Next.js의 기본 ESLint 구성을 포함
```
# .exlintrc.json
{
  "extends": "next/core-web-vitals"
}
```
2. Base: 기본 ESLint 구성
```
#.eslintrc.json

{
  "extends": "next"
}
```
3. Cancel: 사용자 정의 ESLint 구성을 설정 계획인 경우 선택
- next.js 개발 종속성으로 eslint 및 eslint-config-next를 자동 설치하고 구성을 포함하는 .eslintrc.json 파일 생성
- ESLint 실행 오류 포착시 린트 실행
## ESLint Config
- 기본 구성에는 next.js에서 사용 가능한 최적 Linting 경험을 제공하는 데 필요한 모든 것 포함
- 애플리케이션에 ESLint 구성되어 있지 않은 경우 next lint 사용하여 구성 설정
- 추천 eslint-config-next 설정
1. eslint-plugin-react
2. eslint-plugin-react-hooks
3. eslint-plugin-next
## ESLint Plugin
- [eslint-plugin-next](https://nextjs.org/docs/app/building-your-application/configuring/eslint#eslint-plugin)
### Custom Settings
- rootDir
```
# .eslintrc.json

{
  "extends": "next",
  "settings": {
    "next": {
      "rootDir": "packages/my-app/"
    }
  }
}
```
## Linting Custon Directories and Files
```
# next.config.js

module.exports = {
  eslint: {
    dirs: ['pages', 'utils'], // Only run ESLint on the 'pages' and 'utils' directories during production builds (next build)
  },
}
```
- 특정 디렉토리 Lint하기
```
next lint --dir pages --dir utils --file bar.js
```
## 규칙 비활성화
```
# .eslintrc.json

{
  "extends": "next",
  "rules": {
    "react/no-unescaped-entities": "off",
    "@next/next/no-page-custom-font": "off"
  }
}
```
### Core Web Vitals
- 엄격한 규칙시 활성화 
```
# .eslintrc.json

{
  "extends": "next/core-web-vitals"
}
```
## Prettier 사용
```
npm install --save-dev eslint-config-prettier
yarn add --dev eslint-config-prettier
```
- ESLint Config에 prettier 추가
```
{
  "extends": ["next", "prettier"]
}
```
# Environment Variables
- 환경변수에 대한 기본 제공 지원
- .env.local을 사용하여 로드
- NEXT_PUBLIC_을 접두사로 붙여 브라우저에 노출
## Env 로드
```
# .env.local

DB_HOST=localhost
DB_USER=myuser
DB_PASS=mypassword
```
- 사용시 process.env.~로 사용
### 다른 변수 참조하기
```
# .env.local

TWITTER_USER=vercel
TWITTER_URL=https://twitter.com/$TWITTER_USER
```
## 브라우저 환경변수 노출
- 기본적으로 Node.js 환경에서 사용되므로 브라우저 노출되지 않는다.
- 브라우저 노출시 NEXT_PUBLIC_을 붙여야한다.
```
NEXT_PUBLIC_ANALYTICS_ID=abcdefghijk
```
## 환경변수 로드 순서
1. process.env
2. .env.$(NODE_ENV).local
3. .env.local(NODE_ENV테스트일때 체그 하지 않음)
4. .env.$(NODE_ENV)
5. .env
