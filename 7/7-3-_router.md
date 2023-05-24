# 7주차 3강\_Router

## External한 React Router

React에서는 아래 코드와 같이 React Router를 통해 url과 component, 데이터 로딩 및 조작 함수, 에러용 component를 연결할 수 있습니다.

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
    </Routes>
   </main>
   <Footer />
  </div>
 );
}
```

하지만 이 경우 route 정보가 모듈화 되어있지 않아, 다른 react 함수나 테스트 코드 등에서 재사용할 수가 없습니다. 그래서 재사용성을 높이고, 관심사 분리를 위해 route도 external하게 만들 수 있습니다.

1. Router 함수 생성
우선 router 부분을 별도의 함수로 빼낼 수 있습니다. 이 경우 Page를 별도의 component로 빼면 route 부분을 재사용 할 수 있습니다.

```tsx
import { Routes, Route } from 'react-router-dom';

function Page(){
 return(
  <Routes>
   <Route path="/" element={<HomePage />} />
   <Route path="/about" element={<AboutPage />} />
  </Routes>
 );
};

function App() {
 return (
  <div>
   <Header />
   <main>
    <Page />
   </main>
   <Footer />
  </div>
 );
}
```

하지만 이 경우 route는 그 객체만 존재하는 것이 아니라 UI를 그리는 React에 속해있게 됩니다. 따라서 이보다는 route를 객체화하여 사용하는 것이 더 좋습니다. 이를 위해 createBrowserRouter, RouterProvider 라이브러리를 사용하게 됩니다.

1. route 객체 생성 및 routing
우선 routes를 객체로 만듦니다. 그리고 createBrowserRouter 라이브러리를 통해 route object로 만듦니다. 마지막으로 RouterProvider 라이브러리의 route Props로 만들어진 route object를 전달하여 사용합니다. 이렇게 되면 routes는 React와 완전히 분리됩니다.(관심사의 분리)

```tsx
import { createBrowserRouter, RouterProvider } from 'react-router-dom';

const routes = {
 [
  {path: "/", element: <HomePage />},
  {path: "/about", element: <AboutPage />},
 ]
};

const route = createBrowserRouter(routes);

function App() {
 return (
 <RouterProvider route={route} />
 );
}
```

이제 여기에 Header, Footer와 같은 Layout 요소를 넣어주면 됩니다. 이 Layout 요소도 따로 분리하여 관리할 수 있습니다. 이를 위해서는 Outlet 라이브러리를 사용합니다.

1. 페이지 Layout 분리
우선 Layout이 담긴 컴포넌트를 생성합니다. routes 객체를 통해 컴포넌트가 교체될 자리에 Outlet을 넣습니다. 그리고 routes 객체에 element의 값으로 생성한 Layout 컴포넌트를 연결하고, chlidren에 path와 element가 연결된 객체 배열을 넣습니다. 그러면 랜더링 시 현재 path와 chlidren의 path가 일치하는 컴포넌트가 Outlet 위치에 삽입됩니다.

```tsx
import { createBrowserRouter, RouterProvider } from 'react-router-dom';

function Layout(){
 // main tag는 삭제함
 return(
   <div>
   <Header />
   <Outlet />
   <Footer />
  </div>
 );
};

const routes = {
 element: <Layout />,
 chlidren: [
  {path: "/", element: <HomePage />},
  {path: "/about", element: <AboutPage />},
 ],
};

const route = createBrowserRouter(routes);

function App() {
 return (
 <RouterProvider route={route} />
 );
}
```

1. 파일 분리
마지막으로 지금까지 만들어진 Layout, routes, App을 각각의 파일로 분리해 import하는 형태로 만들면 External한 React Router가 완성됩니다.

## 강의를 들으면서 든 생각

이번 강의를 통해 우리 팀(프론트 팀)에서 React를 공부하는 사람들이 늘어나자 백엔드팀에서 환호를 하며 좋아했는지 느낄 수 있었다. 지금 회사에서는 백엔드에서 api를 만들 뿐만 아니라 ajax를 통해 데이터를 부르고 가공하는 과정까지 모두 진행하고 있다. 또한 controller도 그들이 다루고 있다. 그런데 React Router를 배우니 Spring의 controller의 역할과 ajax와 같이 api를 통해 데이터를 로딩하고 가공하는 모든 과정을 클라이언트에서 쉽게 처리할 수 있음을 알 수 있었다. 강의 듣는 내내 백엔드팀 전원이 너무 행복해하며 드디어 프론트와 백의 분리가 가능하다고 신나했던 이유를 배운 느낌이었다.

### 추가로 공부할 것

전체 흐름과 그렇게 진행하는 이유는 알았으나, 강의 내용 중 `<Page />`를 사용하는 것이 아니라 route 객체를 만들어서 사용하는 것이 좋은 이유를 아직 잘 모르겠다.
