# 4주차 3강_React의 Hook

## 설명

### React Hook

React Hook은 기존의 Class를 사용하지 않고 state와 state를 조작하는 React의 여러 기능들을 편리하게 사용하기 위해 만들어진 라이브러리입니다. 기존 Class 방식은 this를 이용한 코드로 인해 정확하게 원하는 내용만 변경되도록 개발이 필요했으며 코드 가독성이 떨어졌습니다. 또한 컴포넌트 간에 state 관련 로직을 재사용할 수 없었으며, 이를 해결하기 위해 고안된 여러 방법들은 이해하기 어려운 코드를 생산해냈습니다. 그래서 React에서는 useState, useEffect 등 사용 이유가 명확한 이름을 가진 hook을 사용하여 필요한 state와 이를 조작하는 함수들을 정의할 수 있도록 만들었습니다. Hook은 이전 버전의 react 코드와 완벽하게 호환되어 코드 수정 없이 필요한 부분에만 적용할 수 있습니다.
hook이 생기면서 function component만 사용하여 react app을 구성할 수 있게 되었습니다. 그리고 js에서 function은 first-class object이기에 이를 할당해서 매개변수로 넘겨주거나 return할 수 있습니다. 그 덕분에 자유롭게 컴포넌트 간에 state를 공유하고 재사용할 수 있게 되었습니다.

#### Hook 사용 규칙

1. 같은 hook을 여러 번 호출함으로써 this를 사용하지 않고도 독립적인 state와 함수를 정의할 수 있습니다. 
2. Hook은 if, for, 중첩 함수와 같은 함수 내에 있는 block 내부에 호출될 수 없습니다. 반드시 최상위에 있는 function body에서 호출되어야 합니다.
3. 일반 js 함수가 아닌 React 함수에서만 사용 가능합니다.

#### 대표적인 Hook의 종류(작성 중)

- useState: state와 setState 함수를 return합니다. 이를 통해 서로 영향을 주지 않는 state들을 정의하고 값을 업데이트 할 수 있습니다. 값이 업데이트 될 때마다 리랜더링됩니다.

```jsx
const [count, setCount] = useState(0);
```

- useEffect: 
- useContext
- useRef
- useLayoutEffect

- **React StrictMode**: strictMode는 개발 시 오류를 잡아낼 수 있도록 하는 모드로 strictMode가 선언된 부분은 2번 실행되면서 서로 비교하는 방식으로 오류 여부를 판별합니다. 따라서 hook 사용 시 의존성 배열을 활용해 1회만 사용되도록 하여도 최초에 2번 실행되게 됩니다.

## 강의를 들으면서 든 생각

4강 데브노트에 작성함

### 추가로 공부할 것

추상화
