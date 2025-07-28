# Routing

App router는 app 이라는 새로운 디렉토리에서 작동한다.

app 디렉토리는 pages 디렉토리와 함께 작동하여 점진적인 도입을 허용한다.

app 내부의 컴포넌트는 React Server Components이고, Client Components도 사용할 수 있다.

## 폴더와 파일의 역할

폴더 : 경로를 정의하는데 사용

파일 : 경로 세그먼트에 대해 표시되는 UI를 생성하는데 사용

### file 규칙

Next.js에서 중첩 경로에서 특정 동작을 가진 UI를 생성하기 위해 일련의 특수파일을 제공한다.

특수파일에는 .js, .jsx, .tsx 파일 확장자를 사용할 수 있다.

|                                                                                                                |                                                            |                                                                       |
| -------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------- | --------------------------------------------------------------------- |
| [`layout`](https://nextjs-ko.org/docs/app/building-your-application/routing/layouts-and-templates#layouts)     | 세그먼트와 자식에 대한 공유 UI                             | 여러 경로간에 공유되는 UI                                             |
| 중첩될 수 있다.                                                                                                |
| [`page`](https://nextjs-ko.org/docs/app/building-your-application/routing/pages)                               | 경로의 고유한 UI로 경로를 공개적으로 접근 가능하게 만든다. | 모든 페이지에서 고유로 갖는 페이지. index.ts 와 같다고 생각하면 된다. |
| [`loading`](https://nextjs-ko.org/docs/app/building-your-application/routing/loading-ui-and-streaming)         | 세그먼트와 자식에 대한 로딩 UI                             |                                                                       |
| [`not-found`](https://nextjs-ko.org/docs/app/api-reference/file-conventions/not-found)                         | 세그먼트와 자식에 대한 Not Found UI                        |                                                                       |
| [`error`](https://nextjs-ko.org/docs/app/building-your-application/routing/error-handling)                     | 제그먼트와 자식에 대한 오류 UI                             |                                                                       |
| [`global-error`](https://nextjs-ko.org/docs/app/building-your-application/routing/error-handling)              | 글로벌 오류 UI                                             |                                                                       |
| [`route`](https://nextjs-ko.org/docs/app/building-your-application/routing/route-handlers)                     | 서버 사이드 API 엔드포인트                                 |                                                                       |
| [`template`](https://nextjs-ko.org/docs/app/building-your-application/routing/layouts-and-templates#templates) | 특수한 재렌더링된 레이아웃 UI                              |                                                                       |
| [`default`](https://nextjs-ko.org/docs/app/api-reference/file-conventions/default)                             | 병렬 경로에 대한 fallback UI                               |                                                                       |

특수 파일에 정의된 React 컴포넌트는 특정 계층 구조로 렌더링된다.

## Linking and Navigating

- <Link> 컴포넌트 사용
- useRouter 훅 사용(클라이언트 컴포넌트)
- redirect 함수 사용(서버 컴포넌트)
- 네이티브 History API 사용

## Error and Handling

에러는 예상된 에러와 예상치 못한 예외 두 가지 범주로 나눌 수 있다.

### 예상된 에러를 반환값으로 모델링 :

서버 액션에서 예상된 에러를 try/catch로 처리하는 것을 피한다. 에러를 throw 하는 대신 반환 값으로 모델링 해야한다. 그 다음 useActionState 훅에 액션을 전달하고 반환된 state를 사용하여 에러 메세지를 표시할 수 있다.

정상적인 애플리케이션 운영 중에 발생할 수 있는 에러. 이러한 에러는 명시적으로 처리하고 클라이언트에 반환해야 한다.

### 예상치 못한 에러는 에러 경계로 처리 :

정상적인 애플리케이션 흐름 중에 발생해서는 안되는 버그나 문제를 나타내는 예기치 않은 에러.

이러한 에러는 throw 하여 에러 경계에서 처리해야 한다.

**일반적인 경우** : 루트 레이아웃 아래에서 발생한 예기치 않은 에러는 error.js 로 처리하

**선택적인 경우** : 중첩된 error.js 파일을 사용하여 세분화된 예기치 않은 에러를 처리

**드문 경우** : 루트 레이아웃에서 발생한 예기치 않은 에러는 global-error.js 로 처리한다.

error.tsx 및 global-error.tsx 파일을 사용하여 에러 경계를 구현하고 예상치 못한 에러를 처리하며 대체 UI를 제공한다.
