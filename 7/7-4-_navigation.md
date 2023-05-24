# 7주차 4강\_Navigation

## Path가 이동되어도 전체 페이지가 재로딩되지 않도록 하는 법

### Web APIs를 이용하는 방법

Web APIs 중 하나인 History 인터페이스를 활용해 현재 페이지를 불러온 탭이나 프레임의 방문 기록, 즉 브라우저 세션 기록을 확인하고 조작할 수 있습니다. 이를 활용하여 페이지 이동 버튼(이후 a tag를 줄여 'a'라고 표기)을 클릭했을 경우 event를 막고 `history.pushState(state, title, url)`함수로 브라우저 세션 기록 스택에 상태값을 추가하는 방식으로 임시 구현이 가능합니다.

```tsx
const handleClick = (event) => {
  event.preventDefault();
  const state = {};
  const title = '';
  const url = '/about';
  window.history.pushState(state, title, url);
};

<a href="/" onClick={handleClick}>home</a>
```

### React Router를 이용하는 방법

1. Link와 NavLink
Link와 NavLink는 모두 React Router에 있는 element로, path가 변경되었을 때 해당되는 component만 리랜더링 될 뿐 화면이 전환되지는 않도록 합니다. Link를 사용할 경우에는 단순히 페이지 전환 대신 리랜더링을 유도하는 기능만 작동하고, NavLink를 쓰면 현재 선택된 항목에 active class가 들어가면서 css나 그 외 액션을 잡기가 쉬워집니다.

```tsx
import { Link, NavLink } from 'react-router';

<Link to="/">home</Link>
<NavLink to="/">home</NavLink>
```

1. redirect를 하는 방법_Navigate, useNavigate
웹 사이트에서는 기획에 따라 logout, login과 같이 일부 페이지의 경우 해당 페이지에 들어갔다가 redirect로 main이나 다른 페이지로 이동을 시켜야 하는 경우가 있습니다. 이 땐 React Router의 Navigate나 useNavigate를 활용할 수 있습니다.

- Navigate는 이동된 페이지의 return에 사용하는 element입니다. 따라서 Link나 NavLink, 혹은 단순히 path와 element를 연동한 모든 경우에 사용 가능합니다.

```tsx
// logout.tsx 파일
import { Navigate } from 'react-router';

export default function LogoutPage() {
  return (
    <Navigate to="/" />
  );
}

// Header.tsx 파일
<NavLink to="/logout">logout</NavLink>
```

- useNavigate는 함수이기에 변수로 받은 뒤 해당 변수를 사용하면 됩니다. 함수를 통해 바로 navigation을 사용할 수 있기 때문에 컴포넌트를 만들거나 path와의 연결을 위해 route를 사용할 필요가 없습니다.

```tsx
import { useNavigate } from 'react-router';

const navigate = useNavigate();

const handleClickLogIn = () => {
  navigate('/');
};

<button type="button" onClick={handleClickLogIn}>login</button>
```

<details>
<summary>전체 예시 코드</summary>

- Header.tsx 파일

```tsx
import { Link, NavLink, useNavigate } from 'react-router-dom';

export default function Header() {
  const handleClick = (event) => {
    event.preventDefault();
    const state = {};
    const title = '';
    const url = '/about';
    window.history.pushState(state, title, url);
  };

  const navigate = useNavigate();

  const handleClickLogIn = () => {
    navigate('/');
  };

  return (
    <header>
      <nav>
        <ul>
          {/* Web APIs history 사용 */}
          <li>
            <a href="/" onClick={handleClick}>home</a>
          </li>
          <li>
            <a href="/about" onClick={handleClick}>about</a>
          </li>
          <hr />
          {/* Link와 NavLink */}
          <li>
            <Link to="/">home</Link>
          </li>
          <li>
            <Link to="/about">about</Link>
          </li>
          <li>
            <NavLink to="/">home</NavLink>
          </li>
          <li>
            <NavLink to="/about">about</NavLink>
          </li>
          <hr />
          {/* redirect를 하는 방법 */}
          <li>
            <NavLink to="/logout">logout</NavLink>
          </li>
          <li>
            <button type="button" onClick={handleClickLogIn}>login</button>
          </li>
        </ul>
      </nav>
    </header>
  );
}
```

- LogoutPage.tsx 파일

```tsx
import { Navigate } from 'react-router';

export default function LogoutPage() {
  // useEffect로 액션 기록 가능
  return (
    <Navigate to="/" />
  );
}
```

- routes.tsx 파일

```tsx
import Layout from './components/Layout';
import HomePage from './pages/HomePage';
import AboutPage from './pages/AboutPage';
import LogoutPage from './pages/LogoutPage';

const routes = [
  {
    element: <Layout />,
    children: [
      { path: '/', element: <HomePage /> },
      { path: '/about', element: <AboutPage /> },
      { path: '/logout', element: <LogoutPage /> },
    ],
  },
];

export default routes;
```

</details>

## 강의를 들으면서 든 생각

routing을 하면 react의 장점인 리랜더링 효과가 약해지는 건가 생각했는데, 역시 사람들은 다 만들어놨구나 하는 생각이 오늘도 들었다.

### 추가로 공부할 것

- 쿠키와 세션
