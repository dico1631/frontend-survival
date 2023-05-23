# 7주차 2강\_Routes

- 라우터란? 논리적 또는 물리적으로 라우팅을 해주는 장치

## React Router

React Router는 클라이언트(React app) 사이드 라우팅을 할 수 있도록 react에서 제공하는 라이브러리입니다. 이를 통해 path와 component(= element) 그리고 fetch와 같이 랜더링에 필요한 데이터 함수를 연결할 수 있습니다. React Router는 nested route를 사용하여 app의 레이아웃과 data를 매치함으로써 단순하고 선언적인 코드를 짤 수 있도록 합니다.

### 기본 사용법

> [Routes](https://reactrouter.com/en/main/components/routes)
> [Route](https://reactrouter.com/en/main/route/route)

- `<Routes>`는 app의 랜더링되는 곳 어디서든 현 위치를 기준으로 하위의 route와 component를 매치하는 역할을 합니다. 하위에는 `<Route>`가 들어갈 수 있습니다.
- `<Route>`는 url과 component, 데이터 로딩 및 조작 함수를 연결합니다. 매개변수로는 path, element, loader 함수, action 함수, errorElement를 받습니다.
  - path: url
  - element: url과 연결할 component
  - loader 함수: 화면 랜더링 전에 동작할 데이터 로딩 함수
  - action 함수: 로딩된 데이터를 가공하는 함수
  - errorElement: 동작 중 오류가 발생 시 랜더링할 component

<details>

<summary> `<Route>` 구조 </summary>

  ```jsx
  const router = createBrowserRouter([
    {
      // it renders this element
      element: <Team />,

      // when the URL matches this segment
      path: "teams/:teamId",

      // with this data loaded before rendering
      loader: async ({ request, params }) => {
        return fetch(
          `/fake/api/teams/${params.teamId}.json`,
          { signal: request.signal }
        );
      },

      // performing this mutation when data is submitted to it
      action: async ({ request }) => {
        return updateFakeTeam(await request.formData());
      },

      // and renders this element in case something went wrong
      errorElement: <ErrorBoundary />,
    },

  ]);
  ```

</details>

```tsx
import { Routes, Route } from 'react-router-dom';

function App() {
 return (
  <div>
   <Header />
   <main>
    <Routes>
     <Route path="/" element={<HomePage />} />
     <Route path="/about" element={<AboutPage />} />
      <Route
        path="dashboard"
        element={<Dashboard />}
        loader={({ request }) =>
          fetch("/api/dashboard.json", {
            signal: request.signal,
          })
        }
      />
    </Routes>
   </main>
   <Footer />
  </div>
 );
}
```

### Browser Router

웹 브라우저에 화면을 랜더링하기 위한 app이기에 BrowserRouter 세팅이 필요합니다. `<BrowserRouter>`은 브라우저 주소창의 url을 사용하여 현 위치를 저장하고, 브라우저에 내장된 히스토리 스택을 탐색할 수 있게 합니다.

> [BrowserRouter](https://reactrouter.com/en/main/router-components/browser-router)

```tsx
import { BrowserRouter } from 'react-router-dom';

root.render((
 <BrowserRouter>
  <App />
 </BrowserRouter>
));
```

### Route

### Memory Router

`<MemoryRouter>`는 내부적으로 배열에 위치를 저장합니다. 브라우저의 히스토리 스택과 같은 외부 소스에 연결되지 않습니다. 따라서 테스트와 같이 히스토리 스택을 완벽하게 제어해야 하는 시나리오에 이상적입니다.

> [MemoryRouter](https://reactrouter.com/en/main/router-components/memory-router)

```tsx
describe('App', () => {
 function renderApp(path: string) {
  render((
   <MemoryRouter initialEntries={[path]}>
    <App />
   </MemoryRouter>
  ));
 }

 context('when the current path is “/”', () => {
  it('renders the home page', () => {
   renderApp('/');

   screen.getByText(/Hello/);
  });
 });

 context('when the current path is “/about”', () => {
  it('renders the about page', () => {
   renderApp('/about');

   screen.getByText(/About/);
  });
 });
});
```

## 강의를 들으면서 든 생각

### 추가로 공부할 것

히스토리 스택
