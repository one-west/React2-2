# React2 3-2반 201930129 정한서

## 11주차 메모 24-11-06

### UI 라이브러리

- UI 라이브러리, 프레임워크, 유틸리티는 필수 X

- 생산성 향상 및 UI 일관성

- **chakra UI**

  - 버튼, Modal, 입력 등 다양한 내장 컴포넌트 제공

  - dark mode, light mode 지원

  - 타입스크립트로 작성되어 있음

  ```sh
  npm i @chakra-ui/react @emotion/react
  npx @chakra-ui/cli snippet add
  ```

- **TailwindCSS**

  - 다른 프레임워크와는 다르게 CSS 규칙만을 제공

  - JS 모듈이나 react 컴포넌트를 제공하지 않기 때문에 필요한 경우 직접 만들어서 사용해야 함

  - 변수값을 조정하여 개성있는 디자인을 만들 수 있음. 디자인 자유도가 높다.

  - dark mode 및 light mode를 쉽게 적용할 수 있다.

  - 빌드 시점에 사용하지 않는 클래스는 제거 되기 때문에 높은 수준의 최적화를 지원

  - CSS 클래스의 접두사를 활용해서 모바일, 데스크톱, 태블릿 화면에서 원하는 규칙을 지정할 수 있음

    - 예시코드 `<div className="sm:hidden md:flex lg:inline-block"></div>`

    > sm => @media (min-width: 640px) { ... }  
    > md => @media (min-width: 768px) { ... }  
    > lg => @media (min-width: 1024px) { ... }  
    > xl => @media (min-width: 1280px) { ... }  
    > 2xl => @media (min-width: 1536px) { ... }

  - 현재는 [TailwindCSS](https://tailwindcss.com/)와 [TailwindUI](https://tailwindui.com/)를 지원함

- **Headless UI**

  - TailwindCSS를 만든 Tailwind Labs 팀의 무료 오픈소스 프로젝트

  - TailwindCSS는 웹 컴포넌트 안에서 사용할 수 있는 CSS클래스만 제공함

  - Headless UI는 CSS클래스를 제공하는 것이 아닌 동적 컴포넌트만 제공한다.

  ```sh
  npm install @headlessui/react
  ```

## 10주차 메모 24-10-30

### CSS와 내장 스타일링 메서드

- **Styled JSX**

  - Styled JSX는 CSS-in-JS 라이브러리이다.

  - 내장 모듈이기에 설치가 필요 없음

    ```jsx
    "use client";

    export default function StyledJsx2() {
      return (
        <>
          <button className="button">버튼</button>
          <span>Span Tag</span>
          <style jsx>
            {`
              span {
                background-color: blue;
                color: white;
                font-size: 1rem;
              }
            `}
          </style>
        </>
      );
    }
    ```

  - 단점

    - IDE나 코드 편집기 등 개발 도구에 대한 지원이 부족

    - 문법 하이라이팅, 자동 완성, 린트(lint)기능을 제공 X

    - 코드 내에서 CSS에 대한 의존성이 점점 커지기 때문에 앱 번들도 커지고 느려짐

- **CSS Module**

  - CSS-in-JS의 단점을 보완하기 위한 방법이다.

  - .module.css 로 끝나는 파일에서 CSS클래스를 가져와서 사용

  - 변환한 객체에서 모든 키는 클래스 이름을 의미함

  - 클래스들은 컴포넌트 스코프를 가진다.

  - 생성된 HTML 태그를 보면 class 가 고유한 값을 갖는다.

    ```jsx
    /* styles/globals.css */

    .foo {
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      align-items: center;
      gap: 32px;
      grid-row-start: 2;
      color: yellow;
    }
    ```

    ```jsx
    /* layout.js */

    import "./styles/globals.css";
    ```

    ```jsx
    /* page.js */

    "use client";

    export default function Root() {
      return (
        <div className="foo">
          <h1>Root Page</h1>
        </div>
      );
    }
    ```

  - CSS Module 상속

    ```jsx
    /* styles/my.module.css */

    .main {
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      align-items: center;
      gap: 32px;
      grid-row-start: 2;
      color: red;
    }

    .main1 {
      composes: main;
      color: #cacaff;
    }
    ```

    ```jsx
    /* page.js */

    import styles from "./styles/my.module.css";

    export default function Root() {
      return (
        <div className={styles.main1}>
          <h1>Root Page</h1>
        </div>
      );
    }
    ```

- **SASS**

- Next에서 기본으로 지원하는 전 처리기

- 단 패키지 설치가 필요함 ( npm install sass )

- SASS 및 SCSS(Sassy CSS) 문법으로 CSS Module을 만들고 사용할 수 있다.

- styles/Home.module.css 파일 이름을 styles/Home.module.scss로 바꿔주면 된다.

  ```scss
  // styles/foo.module.scss

  $foo: red;

  .bar {
    font: 500;
    color: aqua;
  }
  ```

  ```jsx
  import styles from "./styles/foo.module.scss";

  export default function Root() {
    return (
      <>
        <div className="foo">
          <h1>Home_foo</h1>
        </div>

        <div className={styles.bar}>
          <h1>Home_foo1</h1>
        </div>
      </>
    );
  }
  ```

## 9주차 메모 24-10-23

### 누적 레이아웃 이동 (CLS: Cumulative Layout Shift)

- 정적 자원 중 이미지 파일은 SEO에 많은 영향을 미친다.

- 다운로드 시간이 많이 걸리고, 렌더링 후에 레이아웃이 변경되는 등 UX에 영향을 미친다.

- Image 컴포넌트를 사용하면 해결

- lazy loading : 이미지 로드 시점을 필요할 때까지 지연시키는 기술

- 이미지 사이즈 최적화로 사이즈를 1/10이하로 줄여줌

- placeholder를 제공

![예시 이미지](https://www.debugbear.com/public/docs/cumulative-layout-shift/cls-filmstrip.png)

### Image Component

#### local 방식

- 예시 코드

```jsx
import Image from "next/image";
import foo from "/public/images/leaf-6760484_1920.jpg";

export default function About() {
  return (
    <>
      <h1>About page</h1>
      {/* 경로 방식 */}
      <Image
        src="/images/corn-9064747_640.jpg"
        alt="옥수수"
        width={400}
        height={500}
      />
      <Image
        src="/images/corn-9064747_640.jpg"
        alt="옥수수"
        width={400}
        height={500}
        layout="responsive"
      />
      {/* import 방식 */}
      <Image src={foo} alt="단풍" width={400} height={500} />
    </>
  );
}
```

- Image 컴포넌트 사용 시 주의 사항

  - width, height는 필수이다. (layout="fill"을 사용 시에는 생략)

  - layout="responsive" 는 브라우저 크기에 맞게 가변한다.

### Remote 방식

- Pixabay와 같은 외부 이미지를 사용하려면 next.config.mjs 설정이 필요

- 수정된 코드

```mjs
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
  images: {
    remotePatterns: [
      {
        protocol: "https",
        hostname: "cdn.pixabay.com",
      },
    ],
  },
};

export default nextConfig;
```

- 예시 코드

```jsx
import Image from "next/image";

export default function About() {
  return (
    <>
      <h1>About page</h1>
      {/* Remote 방식*/}
      <Image
        src="https://cdn.pixabay.com/photo/2023/11/03/12/22/toadstool-8362901_1280.jpg"
        width={300}
        height={500}
        alt="버섯"
      />
    </>
  );
}
```

### 코드 구성과 데이터 불러오기

- 프로젝트를 시작할 때 확장과 복잡도에 대비 해야한다.

- 코드를 더 효율적으로 구성하기 위해 **아토믹 디자인 원칙**에 따라 디렉토리를 구성한다.

  - atoms : 가장 기본적인 컴포넌트 관리

  - molecules : atoms에 속한 컴포넌트 여러 개를 조합하여 복잡한 구조로 만든 컴포넌트 관리

  - organisms : molecules와 atoms를 섞어서 더 복잡하게 만든 컴포넌트 관리

  - templates : 위의 모든 컴포넌트를 어떻게 배치 결정해서 사용자가 접근할 수 있는 페이지

## 8주차 메모 24-10-16

- 중간고사

## 7주차 메모 24-10-25

**24-10-09 한글날 보강 휴무로 인한 보강**

### Axios로 api 연동

## 6주차 메모 24-10-02

### 동적 라우팅 상세

- [slag] (단일 동적 경로 : Dynamic Route Segment)

  - 단일 동적 세그먼트를 처리

  - 파일구조 : app/posts/[slug]/page.js

  ```jsx
  export default function Post({ params }) {
    const { slug } = params;

    return <div>Post: {slug}</div>;
  }
  ```

- [...slag] (다중 동적 경로 : Catch-All Dynamic Route)

  - 0개 이상의 경로 세그먼트를 배열로 처리

  - 세그먼트 값이 없으면 404 Bad Request 에러 발생

  - 파일구조 : app/posts/[...slug]/page.js

  ```jsx
  export default function Post({ params }) {
    const { slug } = params;

    return <div>Slug segments: {slug ? slug.join("/") : "No slug"}</div>;
  }
  ```

- [[...slag]] (선택적 동적 경로 : Optional Catch-All Dynamic Route)

  - 경로를 선택적으로 지정한다. (즉, 경로 세그먼트가 없어도 해당 경로로 접근할 수 있습니다)

  - 파일구조 : app/posts/[[...slug]]/page.js

    ```jsx
    export default function Post({ params }) {
      const { slug } = params;

      if (!slug) {
        return <div>All posts</div>;
      }

      return <div>Slug segments: {slug.join("/")}</div>;
    }
    ```

## 5주차 메모 24-09-25

### Next.js 기초와 내장 컴포넌트

#### 클라이언트와 서버에서의 라우팅 시스템 작동 방식

- Next는 파일시스템 기반의 페이지 라우팅과 앱 라우팅을 함

- 페이지 라우팅은 /pages 디렉토리 안의 `index.js`, `index.jsx`, `index.tsx` 파일에서 export한 React 컴포넌트

- 앱 라우팅은 src/app 디렉토리 안에 `page.js`, `page.jsx`, `page.tsx`의 파일에서 export한 React 컴포넌트

- 동적 라우팅 규칙을 만들려면 페이지 라우팅은 [slug].js 파일, 앱 라우팅은 [slug] 디렉토리가 필요

- [slug].js 는 매개변수로 사용되며 주소창에서 입력하는 값을 모두 받을 수 있다.

- 동적 라우팅 규칙은 중첩도 가능

- 접근 경로를 `~/posts/[data][slug]` 형태로 받을 수 있다.

- 예시 : app/foo/[fooId]/page.jsx (경로)

  ```jsx
  export default async function fooId(props) {
    console.log(props);

    return (
      <h1>
        foo {props.params.fooId} / {props.searchParams.country}
      </h1>
    );
  }
  ```

- URL : http://localhost:3000/foo/2?country=KOR

#### 페이지 간 이동 최적화

- Next.js 가 정적 자원을 제공하는 방법

- 자동 이미지 최적화와 새로운 Image 컴포넌트를 사용한 이미지 제공 최적화 기법

- 컴포넌트에서 HTML 메타 데이터를 처리하는 방법

- \_app.js와 \_document.js 파일 내용 및 커스터마이징 방법

## 4주차 메모 24-10-04

### Page Projcet Layout

**09/18 추석연휴로 인해 10/04 보강**

- \_app.jsx : 서버에 요청할 때 가장 먼저 실행되는 컴포넌트

  - 페이지에 적용할 공통 레이아웃을 선언하는 곳

  - 로직이나 스타일을 선언하여 사용

    ```jsx
    import "@/styles/globals.css";

    export default function App({ Component, pageProps }) {
      return <Component {...pageProps} />;
    }
    ```

- \_document.jsx : 각 페이지에 공통적으로 사용될 html, head, body 안에 들어갈 내용 선언

  - onClick 같은 이벤트나 CSS는 이곳에 선언 X

    ```jsx
    import { Html, Head, Main, NextScript } from "next/document";

    export default function Document() {
      return (
        <Html lang="ko">
          <Head />
          <body>
            <Main />
            <NextScript />
          </body>
        </Html>
      );
    }
    ```

### App Project Layout

- layout.jsx : Page Project에서 사용하던 \_app.jsx와 \_document.jsx를 대체

  - 이 파일을 삭제해고 프로젝트를 실행하면 자동으로 생김

  ```jsx
  export const metadata = {
    title: "Hello Next.js",
    description: "Generated by create next app",
  };

  export default function RootLayout({ children }) {
    return (
      <html lang="en">
        <body className={`${geistSans.variable} ${geistMono.variable}`}>
          {children}
        </body>
      </html>
    );
  }
  ```

- Link component

  - \<a> 태그는 동기식으로 전체가 reload 됨, 외부 링크를 할 때 사용

  - 일반적으로 내부 링크 이동 시에는 사용 X

  - router.push 는 빌드 후, 이동할 주소가 html 상에 노출되지 않기 때문에 SEO에 취약

  - Link 컴포넌트는 빌드 후, \<a> 태그로 자동 변환함

  - \<a> 태그의 장점

    - SEO 최적화

    - prefetch 가능

    - 우 클릭 기능

  - 내부 페이지로 이동할 때 SPA 방식으로 전체 html 중 필요한 부분만 비동기식으로 리렌더링 됨

### 정적 자원 제공

- /public 디렉토리 안에 저장한다. ex) 이미지, 폰트, 아이콘, 컴파일한 CSS, JS 등

- 이미지 파일은 SEO에 많은 영향을 줌

  - 불러오는데 시간이 오래걸림

  - 불러온 후에도 이미지 주변의 레이아웃이 변경되는 등 UX 관점에서 좋지 않은 영향을 줌

- 이를 누적 레이아웃 이동( CLS : Cumulative Layout Shift ) 이라 함

- 만약 사용자가 2번째 문단을 읽고 있었다면, 읽던 곳을 놓칠 수 있음

- Image 컴포넌트를 사용해 CLS 문제 해결

### 자동 이미지 최적화

- Next.js v10 부터 Image 컴포넌트를 사용하면 이미지를 webp 같은 최신 이미지 포맷을 제공

- explorer 같은 오래된 브라우저의 경우 png, jpeg 로 이미지 포맷을 제공

- 이미지 Lazy Loading 처리

  - 페이지가 로드될 때 이미지를 한 번에 다운로드하는 대신 페이지가 스크롤될 때 필요한 이미지만 로드하는 방식

- 디바이스 크기에 알맞는 이미지로 리사이징

  - Image 태그에서 width와 height를 명시할 경우 그에 맞게 생성

  - fill을 사용할 경우 아래처럼 자동으로 srcset이 지정되어 디바이스 별 이미지 사이즈를 설정해준다.

## 3주차 메모 24-09-11

### 파이프라인 문법

```js
console.log(Math.random() * 10);

// 파이프라인 연산자를 사용하면 위 코드를 아래와 같이 바꿀 수 있습니다.

Math.random() |> (x) => x * 10 |> console.log;
```

- ECMAScript 내의 기술 위원회인 TC39에서 이 연산자를 공식적으로 채택하지는 않았지만 바벨 덕분에 사용할 수 있다.

- Next.js 앱에서 파이프라인 연산자를 사용하려면 npm으로 바벨 플러그인을 설치해야 한다.

### Transpile의 동작

- Babel

  - ECMAScript 같은 js 최신 버전이나, TypeScript 이전 버전의 코드로 변환 시키는 Transpile 도구이다.

  - AST(Abstract Syntax Tree)

    - 소스 코드를 추상화된 트리 구조로 나타낸 것

  - Babel의 parser는 js 코드를 컴퓨터가 이해하기 쉽게 변환해준다.

  - 싱글 쓰레드로 실행되기 때문에 속도가 느리다는 단점

- SWC

  - Next 12 이후 Babel에서 SWC로 교체되었다.

  - SWC는 Rust로 작성되어 속도가 훨씬 빠르다

  ※ 속도가 빠른 이유는 Rust에는 WASM(WebAssembly)이라는게 있는데 이게 어셈블리어로 되어있다.(저급언어)

### 렌더링 전략

#### 서버 사이드 렌더링(SSR)

- APM을 사용하는 웹 페이지 생성

- 자바스크립트 코드 적재되면 동적으로 페이지 내용을 렌더링한다.

- Next.js도 동적으로 페이지를 렌더링할 수 있다.

- 리액트 하이드레이션 :

  - 서버에서 렌더링된 HTML 마크업에 기반하여 클라이언트 측에서 자바스크립트 이벤트와 상태를 연결하는 과정을 말한다.

- 장점

  - 안전한 웹 애플리케이션 : 인증 또는 민감한 작업을 서버에서 수행하고 그 결과만 브라우저에 제공해 위협을 피할 수 있다.

  - 뛰어난 웹 사이트 호환성 : 클라이언트 환경이 오래된 브라우저도 서비스를 제공한다.

  - 뛰어난 SEO : 서버가 렌더링한 HTML을 받기에 웹 크롤러가 페이지를 렌더링할 필요가 없음

- 단점

  - 페이지간의 이동은 CSR에 비해 느리다.

  - 서버 과부하

  - 깜빡임 이슈 (매번 새로고침 해야하기 때문에)

- SSR은 만능이 아니다.

  - 단순히 서버가 SPA(Single Page App)를 렌더링한다고 모든 것이 해결되지 않음.
  - 오히려 자바스크립트 코드를 증가시키며, 애플리케이션이 인터렉티브 할 때까지 걸리는 시간이 단순 클라이언트 렌더링보다 더 길어질 수 있다.

#### 클라이언트 사이드 렌더링(CSR)

- 실제 렌더링이 클라이언트에서 이루어지는 방식

- React 앱을 실행하면 렌더링 시작 전에 빈 화면이 한동안 유지 되는 것이 보임

- **장점**

  - 네이티브 앱처럼 느껴지는 웹 앱

    - 전체 자바스크립트 번들을 다운로드 한다는 것은 렌더링할 모든 페이지가 이미 브라우저에 다운로드 되어 있다는 뜻

    - 다른 페이지로 이동해도 서버에 요청할 필요 없이 바로 이동

    - 페이지를 바꾸기 위해 새로 고침이 없다.

  - 쉬운 페이지 전환

    - 클라이언트에서 네비게이션이 새로고침이 발생하지 않아 UX에 도움이 된다.

  - 지연된 로딩과 성능

    - 초기 로딩 이후 빠른 웹사이트 렌더링이 가능(웹사이트가 로딩되는 즉시 상호작용 가능

- **단점**

  - 네크워크가 속도가 느린 환경에서는 번들이 모두 다운로드 될 때까지 빈 페이지를 보아야함

  - 검색 로봇에게도 그 내용은 빈 것으로 보인다.

  - 번들을 모두 받을 때까지 검색 로봇이 기다리기는 하지만 성능 점수는 낮다.

#### 정적 사이트 생성(SSG)

- SSG는 일부 또는 전체 페이지를 빌드 시점에 미리 렌더링

**장점**

1. 쉬운 확장 : 정적 페이지는 HTML 파일으므로 CDN을 통해 파일을 제공하거나, 캐시에 저장하기 쉬움

2. 뛰어난 성능 : 빌드 시점에 미리 렌더링하기 때문에 페이지를 요청해도 클라이언트나 서버가 무언가를 처리할 필요가 없음

3. 더안전한 API 요청 : 외부 API를 호출하거나, DB에 접근하거나, 보호해야 할 데이터에 접근할 일이 없다.
   필요한 모든 정보가 빌드 시 포함되어 있기 때문이다.

## 2주차 메모 24-09-04

### Chocolatey

- Chocolatey (약칭 Choco) : 윈도우에서 사용할 수 있는 커맨드 라인 패키지 매니저

  > Linux 커맨드라인 패키지 매니저 : apt(apt-get), yum  
  > Mac 커맨드라인 패키지 매니저 : Homebrew

- 설치 / 업데이트 / 삭제 등 에 사용하는 Windows용 패키지 매니저

- 설치 URL : https://chocolatey.org/install

- 패키지 URL : https://community.chocolatey.org/packages

- 명령어

  ```shell
  # 패키지 목록
  choco search
  choco list

  # 패키지 원격 검색
  choco list 패키지명

  # 패키지 모든 버전 원격 검색
  choco list -a 패키지명
  choco install 패키지이름

  # 무조건 수락
  choco install -y

  # 특정 버전 선택 설치
  choco install 패키지명 --version 버전

  # 패키지 삭제하기
  choco uninstall 패키지명

  # 패키지 업그레이드
  choco upgrade 패키지명
  ```

  - 최신 버전 설치

  ```shell
  nvm install node
  nvm install lts # lts 최신버전
  ```

  - 버전 지정 설치

  ```shell
  nvm install 16.15.1
  nvm install 16 # 16.x 의 마지막 버전

  nvm uninstall <version> # 필요없는 node 버전 삭제하기
  ```

  - node 전환

  ```shell
  nvm use <version>

  nvm current # 현재 사용중인 버전 확인하기
  ```

### nvm

- NVM (Node Version Manager)는 개발 환경에 따라 Node.js의 버전을 변경해야 하는 상황에 대처하기 위해 필요한 모듈이다.

- 일반 소프트웨어 설치하듯이 exe 파일을 받아 일일히 클릭하여 업데이트 하는 것이 아닌, 터미널에서 명령어로 매우 간단하게 노드 버전을 변경할 수 있다.

- nvm-windows는 MIT 라이센스의 오픈 소스로 Go로 작성되었다.

- Node.js v4+에서 지원되기 때문에 기본적인 Node.js는 설치가 되어 있어야 한다.

- 명령어

  ```shell
  # nvm 버젼 확인
  nvm -v

  # 현재 내 노드 버젼 확인
  nvm ls

  # 사용가능한 노드 버젼 확인
  nvm ls available
  ```

### 프로젝트 기본 구조

- Pages Router

  - 기존 13 이전 버전에서의 Next.js는 원래 /pages 폴더 아래에 원하는 페이지 폴더 목록을 만들어 라우팅을 관리했다.

  - pages 폴더 안에 `index.js` 가 기본 페이지가 된다.

  - EX) localhost:3000/about <= pages 폴더 안에 about.js 파일이 있으면 자동으로 라우팅

- App Router

  - App Router는 /app 폴더를 이용하여 라우팅을 설정할 수 있다. 즉, app 폴더가 pages 폴더를 대체한다고 봐도 된다.

  - app 폴더 안에 `page.js` 가 기본 페이지가 된다.

  - EX) localhost:3000/about <= app 폴더 안에 about폴더 안에 `page.js` 파일이 있으면 자동으로 라우팅

### Next 프로젝트 생성

```shell
npx create-next-app@latest
```

- 뭔가 문제 있을 때

```shell
npm cache clean --force
npm i -g creata-next-app
```

- 그래도 안될 때

```shell
node 버전 변경
nvm install 버전
nvm use 버전
```

### 보일러 플레이트

- url : https://github.com/vercel/next.js/tree/canary/examples

- 위 링크에서 필요한 보일러 플레이트를 가지고 와서 사용하면 시간을 줄일 수 있음

## 1주차 메모 24-08-28

### npm vs yarn

- yarn 설치 : npm i -g yarn

- yarn 확인 : yarn -v

- yarn 삭제 : npm uni -g yarn

### Pages Router vs App Router

React로 개발하다 Next를 사용하면 제일 신기한 기능 Router

교재는 Next.js 13.1 버전이기에 Pages Router로 작성되어 있다.

Next.js 13.4 부터 App Router가 stable하게 지원하기 시작했다.

필자는 App Router로 작성을 진행할 예정

#### [Pages Router]

- pages 디렉토리가 root, index.js가 index page

- about.js는 /about, team.js는 /team 경로로 라우팅

- 클라이언트 중심의 라우팅

#### [App Router]

- app 디렉토리가 root, page.js가 index page

- /about/page.js는 /about, /login/page.js는 /login 경로로 라우팅

- 서버 중심의 라우팅

- 번들 사이즈가 작다.
  > ※번들(bundle) : 의존성 관계가 있는 여러 파일을 하나로 결합하는 것

### Next.js 13 vs Next.js 14

- Pages Router => App Router

- Data Fetching :

  - v13까지는 getServerSideProps, getStaticProps 메소드를 이용하여 구현

  - v14부터는 use 메서드를 이용하여 구현하거나, 컴포넌트 내부에서 비동기 함수(async)를 호출하고, await을 사용하여 데이터를 페칭할 수 있습니다

- Tubopack : webpack에서 Tubopack으로 변경

- Tubopack의 장점

  - 빠른 빌드 속도

  - 코드 스플리팅 및 성능 최적화

  - 번들링 과정 간소화

- 이미지 최적화 : 13까지는 **도구** 사용 / 14부터는 **자체적**으로 지원

  - lazy loading : 지연
  - 이미지 사이즈 최적화 : webp 변환
  - placeholder : 영역 확보

- 보안 강화 : XSS 공격에 대한 보호 기능이 강화, 보안 관련 헤더 설정을 더욱 쉽게 할 수 있다.

### Next.js 알아보기

- Next.js는 리액트를 위해 만든 오픈소스 자바스크립트 웹 프레임워크

- 다양한 기능 제공

  - 서버 사이드 렌더링(SSR: Server Side Rendering)

    > 페이지 요청이 발생할 때마다 사전 렌더링  
    > Fetching 요소 적용하여 렌더링

  - 정적 사이드 생성(SSG: Static Site Generation)

    > 미리 만들어 놓은 페이지를 서비스 하기 때문에 속도는 빠르나 한번 생성하고 나면 수정이 불가능하다.

  - 증분 정적 재생성(ISR: Increamental Static Regeneration)
    > SSG의 단점을 보완하였다.  
    > 이미 생성된 페이지를 일정 시간이 지난 후 다시 생성함(최신 데이터로 업데이트)

- CoC : Convention over Configuration
  - 개발자가 해야 할 결정의 수를 줄이면서, 유연성은 잃지 않도록 하는 설계 패러다임
