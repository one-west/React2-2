# React2 3-2반 201930129 정한서

## 3주차 메모

- 파이프라인 문법

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

    - 서버에서 렌더링된 HTML 마크업에 기반하여  클라이언트 측에서 자바스크립트 이벤트와 상태를 연결하는 과정을 말한다.

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

## 2주차 메모

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

-  Node.js v4+에서 지원되기 때문에 기본적인 Node.js는 설치가 되어 있어야 한다.

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

## 1주차 메모

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
    >※번들(bundle) : 의존성 관계가 있는 여러 파일을 하나로 결합하는 것

### Next.js 13 vs Next.js 14

- Pages Router => App Router

- Data Fetching :

    - v13까지는 getServerSideProps, getStaticProps 메소드를 이용하여 구현
 
    - v14부터는 use 메서드를 이용하여 구현하거나, 컴포넌트 내부에서 비동기 함수(async)를 호출하고, await을 사용하여 데이터를 페칭할 수 있습니다

- Tubopack : webpack에서 Tubopack으로 변경

- Tubopack의 장점

    - 빠른 빌드 속도
 
    - 코드 스플리팅 및 성능 최적화
 
    -  번들링 과정 간소화

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
