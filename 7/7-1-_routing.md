# 7주차 1강\_Routing

## 라우팅(Routing)이란?

라우팅이란 네트워크 안에서 통신 데이터를 보낼 때 최적의 경로를 선택하는 과정입니다. 여기서 최적의 경로란 가장 짧은 거리 혹은 가장 짧은 시간에 데이터를 전송할 수 있는 경로를 말합니다. React에서는 각 페이지를 요청했을 때 url에 따라 어떤 컴포넌트가 뿌려질 지를 지정할 수 있습니다(라우팅). 이를 위해서는 HTML DOM API를 활용하여 Window.location의 값을 사용합니다. Window.location에는 현재 요청된 url의 pathname이 값으로 들어있습니다. 이 pathname과 컴포넌트를 연결시켜서 특정 pathname일 경우에만 해당 컴포넌트를 랜더링하도록 만들 수 있습니다.

- React에서 외부 라이브러리 없이 routing 구현

```tsx
function App() {
 const { pathname } = window.location;

 return (
  <div>
   <Header />
   <main>
    {pathname === '/' && <HomePage />}
    {pathname === '/about' && <AboutPage />}
   </main>
   <Footer />
  </div>
 );
}
```

- HTML DOM API: HTMl DOM에 나오는 element의 값을 가져오고, 조작할 수 있도록 하는 API
  - [HTML DOM API](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API)
- Window.location: Window.location 프로퍼티에 접근하면 읽기 전용인 Location 오브젝트를 얻어올 수 있습니다. 이는 현재 도큐먼트의 로케이션에 대한 정보를 담고 있습니다.
  - [Window.location](https://developer.mozilla.org/ko/docs/Web/API/Window/location)
  - Location: Location 인터페이스는 객체가 연결된 장소(URL)를 표현합니다. Location 인터페이스에 변경을 가하면 연결된 객체에도 반영되는데, Document와 Window 인터페이스가 이런 Location을 가지고 있습니다. 각각 Document.location과 Window.location으로 접근할 수 있습니다.
    - [Location](https://developer.mozilla.org/ko/docs/Web/API/Location)

## 강의를 들으면서 든 생각

웹 프로젝트를 하면서 실제로 많이 사용했던 코드와 용어에 대해 다시 한 번 찾아보는 시간이 되었다.
