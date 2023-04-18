# 1주차 3강_React(작성중)

## 키워드

- React란?
- React 컴포넌트
- React 리렌더링
- IoC(Inversion of Control)
- Library vs Framework

## 설명

### React란

사용자 인터페이스를 더 쉽게 만들 수 있도록 해주는 js 라이브러리
interaction이 있는 app을 만드는데 더 수월하게 해준다.

- 참고 자료

[최신 사용법이 있는 문서](https://beta.reactjs.org/)
[공식문서](https://ko.reactjs.org/)
[React 코어 개발자가 쓴 React에 대한 이해를 돕는 글](https://overreacted.io/ko/react-as-a-ui-runtime/)

### React 컴포넌트

react에서는 컴포넌트를 통해 UI를 재사용이 가능한 개별 조각으로 나누고, 각 조각을 개별적으로 확인함.
컴포넌트는 함수형으로 작성.

### React 랜더링 & 리렌더링

#### 랜더링

화면에 그린다는 의미
`react.render(<컴포넌트 />)`를 통해 컴포넌트를 화면에 그림

#### 리랜더링

react는 화면 전체를 다시 랜더링해주는 것이 아니라, 상태값(state)이 변경되었을 때 컴포넌트의 이전 값과 현재 값을 비교해서 **변경된 부분만** 다시 랜더링해서 변경사항을 반영한다.
`useState()`가 return하는 setState 함수(idx 1)는 state(idx 0)를 변경하고, 리랜더링까지 자동으로 시행한다. 매개변수는 state의 초기값이다.
setState 함수는 매개변수로 값을 받거나, 함수를 전달받는다. 값을 받으면 해당 값으로 state를 변경하고, 함수를 받으면 함수 내용에 따라 state를 변경한 뒤 **변경된 컴포넌트만** 리랜더링 한다.

### IoC(Inversion of Control) (미완성)

제어의 역전

### Library vs Framework (미완성)

## 강의를 들으면서 든 생각

지금 회사에서는 react를 쓰지 않고 바닐라 js랑 jquery를 사용하고 있는데, 해당 프로젝튿에 react를 적용하려면 어떻게 해야하는지 찾아봐야겠다는 생각이 든다.
