# Next.js의 Rendering

```
App Router의 Pre-Rendering 전략에 대해 다룬다.
```

Rendering은 작성한 코드를 사용자 인터페이스로 변환하는 과정입니다. React와 Next.js를 사용하면 코드의 일부를 서버 또는 클라이언트에서 렌더링할 수 있는 웹 애플리케이션을 만들 수 있습니다.

## Fundamentals

이해하기 위한 사전 개념

- 애플리케이션 코드가 실행될 수 있는 Enviornment
- 사용자가 애플리케이션을 방문하거나 상호작용할 때 시작되는 Request-Response Lifecycle
- 서버와 클라이언트 코드를 분리하는 Network Boundary

---

### Rendering Environment

렌더링이 될 수 있는 두 가지 환경 : 클라이언트, 서버

- 클라이언트 : 사용자 장치의 브라우저. 서버에 애플리케이션 코드를 요청하고 서버의 응답을 사용자 인터페이스로 변환한다.

- 서버 : 데이터 센서의 컴퓨터를 의미하며, 애플리케이션 코드를 저장하고 클라이언트의 요청을 받아 적절한 응답을 보낸다.

> 개발자들은 서버와 클라이언트에서 코드를 작성할때 다른 프레임워크를 사용했어야 했다. React를 사용하면 같은 언어, 같은 프레임워크를 사용할 수 있다. 이러한 유연성 덕분에 컨텍스트 전환 없이 두 환경 모두에 대해 코드를 작성할 수 있다.

그러나 각 환경에는 고유한 기능과 제약이 있다. 서버와 클라이언트에 대한 코드는 항상 동일하지 않습니다. 데이터 페칭이나 사용자 상태 관리와 같은 특정 작업은 한 환경에서 다른 환경보다 더 적합할 수 있다.

### Request-Response Lifecycle

대체로 모든 웹사이트는 동일한 Lifecycle을 따릅니다.

1. 사용자 액션 : 사용자가 웹 애플리케이션과 상호작용한다. 링크 클릭, 폼 제출, 브라우저 주소창에 URL 직접 입력 등이 해당

2. HTTP 요청 : 클라이언트는 서버에 필요한 리소스가 무엇인지, 어떤 메서드(Get, Post)가 사용되는지, 추가 데이터가 필요한지를 포함하는 HTTP 요청을 보낸다.

3. 서버 : 서버는 요청을 처리하고 적절한 리소스로 응답한다. 이 과정은 라우팅, 데이터 페칭 등의 여러 단계를 거칠 수 있다.

4. HTTP 응답 : 서버는 요청을 처리한 후 클라이언트에게 HTTP 응답을 보낸다. 이 응답에는 상태코드(응답 성공여부와 클라이언트에게 알림)와 요청된 리소스(html, css, static data 등)가 포함된다.

5. 클라이언트 : 클라이언트는 리소스를 파싱하여 사용자 인터페이스를 렌더링한다.

6. 사용자 액션 : 사용자 인터페이스가 렌더리오디면 사용자가 상호작용할 수 있으며, 전체 프로세스가 다시 시작된다.

### Network Boundary

웹 개발에서 Network Boundary는 다양한 환경을 분리하는 개념적 경계선이다. 예를 들어, 클라이언트와 서버 / 서버와 데이터 저장소를 구분합니다.

React에서는 클라이언트-서버 네트워트 경계를 가장 적절한 위치에 배치할 수 있다.

백그라운드에서는 작업이 두 부분으로 나뉜다. 클라이언트 모듈 그래프와 서버 모듈 그래프.
서버 모듈 그래프는 서버에서 렌더링 되는 모든 컴포넌트를 포함하며, 클라이언트 모듈 그래프는 클라이언트에서 렌더링되는 모든 컴포넌트를 포함한다.

모듈 그래프는 애플리케이션의 파일들이 서로 어떻게 의존하는지에 대한 시각적 표현으로 생각할 수도 있다.

React의 "use client" 지시어를 사용하여 경계를 정의할 수 있다. 또한 "use server" 지시어도 있어, 서버에서 일부 계산 작업을 수행하도록 React에 지시할 수 있다.

### Building Hybrid Applications

이 환경에서 작업할 때 애플리케이션의 코드 흐름을 단방향으로 생각하는 것이 도움이 된다.
즉, 응답중에 애플리케이션 코드는 서버에서 클라이언트로 한 방향으로 흐른다.

클라이언트에서 서버에 접근해야 하는 경우, 동일한 요청을 재사용하는 대신 새로운 요청을 서버로 보낸다. 이렇게 하면 컴포넌트를 어디에서 렌더링할지, Network Boundary를 어디에 둘지 쉽게 이해할 수 있다.

# Partial Prerendering

Partial Prerendering은 동일한 경로에서 정적 렌더링과 동적 렌더링의 이점을 결합하는 렌더링 전략이다. PPR을 사용하면 Suspense 경계 안에 동적 컴포넌트를 감쌀 수 있다.
새로운 요청이 들어오면 Next.js는 캐시에서 즉시 정적 HTML 셸을 제공한 다음, 동적 부분을 동일한 HTTP 요청에서 렌더링하고 스트리밍한다.
https://nextjs-ko.org/docs/app/building-your-application/rendering/server-components

## App router, Page router의 핵심 차이

app/ : 모듈화된 레이아웃과 서버 렌더링 최적화 중심(Next.js 13+표준)
pages/ : 클래식 Next.js 방식으로, 모든게 클라이언트 컴포넌트

### App router(app/)

- React Server Component 기반
- 라우팅 방식 : 파일 + 중첩 layout 구성
- 기능 : 서버/클라이언트 컴포넌트 분리, loading.tsx, error.tsx, layout.tsx 지원

### Page Router(pages/)

- Client-side Rendering 기반
- 라우팅 방식 : 단일 페이지 단위
- 기능 : getServerSideProps, getStaticProps 등으로 데이터 패칭

## Next.js 의 Pre-Rendering은 어디서 나온 개념?

Next.js의 Pre-rendering 개념은 React는 기본적으로 Client-Side-Rendering 만 제공하기 때문에, 이를 보완하기 위해 Next.js가 도입한 기능

- React 자체엔 pre-rendering 기능이 없다.
- Next.js가 초기 로딩 속도, SEO, 성능 최적화를 위해 도입한 전략
  -> 페이지를 요청 받기 전에(혹은 요청시) HTML을 미리 생성해서 클라이언트에 전송하는 방식
- Next.js의 렌더링 방식 중 일부가 pre-rendering 이지만, 모든 렌더링 방식이 pre-rendering 은 아니다.

### 종류

#### Static Generation (SG)

- 빌드 시 HTML 생성(getStaticProps, generateStaticParams)
- 빠름, CDN 캐시 가능

#### Server-Side Rendering (SSR)

- 요청시 HTML 생성
- 항상 최신, 느림

### ✅ 렌더링 방식 4가지 요약 (Next.js 기준)

| 렌더링 방식                               | pre-rendering 여부 | 설명                       |
| ----------------------------------------- | ------------------ | -------------------------- |
| **Static Generation (SG)**                | ✅ O               | 빌드 시 HTML 미리 생성     |
| **Server-Side Rendering (SSR)**           | ✅ O               | 요청 시 서버에서 HTML 생성 |
| **Client-Side Rendering (CSR)**           | ❌ X               | 브라우저에서 JS로 렌더링   |
| **Incremental Static Regeneration (ISR)** | ✅ O               | SG + 주기적 재생성         |

### 핵심 요점

- Pre-rendering : HTML을 서버에서 미리 만들어서 보내는 방식(SG, SSR, ISR 포함)
- CSR은 브라우저에서 JS로 동작 -> pre-rendering 이 아님
- Next.js는 모든 렌더링 방식을 제공하고, 필요에 따라 개발자가 선택함.

-> Next.js의 렌더링 방식은, pre-rendering + CSR 을 조합해서 사용하는 아키텍처 이다.

## App Router에서 pre-rendering 기본 동작

- app/page.tsx, app/layout.tsx는 기본적으로 서버 컴포넌트 -> SSR 처럼 작동
- 특별히 설정하지 않으면 모든 페이지는 pre-render됨

#### 자동 pre-render 조건

- 서버 컴포넌트만 있으면 Static Rendering 적용
- 동적 데이터(fetch 등) 를 사용하면 자동으로 SSR 적용됨
- 동적 경로/params 쓸 경우, generateStaticParams 로 SG 가능

### 렌더링 방법 결정 조건

기본은 자동 결정이고, 원하면 명시적으로 렌더링 방식을 제어할 수 있다.

- fetch() 사용 방식, dynamic 설정, generateStaticParams 유무 등을 보고 Next.js가 알아서 SG/SSR/ISR 중 하나로 렌더링 방식 결정

```
// 자동 static (기본 캐시 사용)
await fetch('https://api.com', {cache : 'force-cache'})

// 자동 SSR (실시간 데이터)
await fetch("https://api.com", {cache : "no-store"})
```

- 필요 시 명시 설정도 가능

```
export const dynamic = 'force-static'
export const dynamic = 'force-dynamic'
export const revalidate = 60(ISR)
```

> 자동 결정하지만, 명시적으로 강제적으로 설정할 수 있다.
