# 6주차 1강_External Store

> React에서 화면을 뿌려주는 UI 영역과 비즈니스 로직 부분을 분리하여 Layered Architecture 설계를 하는 방법과 그렇게 하는 이유에 대해 배워보자.

일반적으로 모든 기능과 로직을 하나로 합쳐놓는 것보다 쪼개진 모듈으로 블럭쌓기를 해서 프로그래밍을 하는 것이 지향됩니다. 모듈형으로 개발을 했을 경우 모듈을 재사용하기 쉽기에 간결한 코드를 작성하기 쉽고, 자연히 가독성 향상과 원활한 유지보수를 기대할 수 있기 때문입니다. React에서도 이를 위해 화면에 랜더링되는 모든 부분을 하나의 return에 모아두기보다는 component로 분리하여 상위 컴포넌트와 하위 컴포넌트가 있는 형태로 개발됩니다. 그렇다면 화면에 출력되는 component 외에 모듈형으로 만들기에 좋은 다른 부분은 없을까요? 이번 강에서는 모듈을 만드는 기본 원칙과 component 외에 기능에 따라 모듈을 분리하는 방법에 대해 배워봅시다.

## 관심사의 분리

> 관심사 분리(separation of concerns, SoC)는 컴퓨터 프로그램을 구별된 부분으로 분리시키는 디자인 원칙으로, 각 부문은 개개의 관심사를 해결한다. [위키백과_관심사 분리](https://ko.wikipedia.org/wiki/%EA%B4%80%EC%8B%AC%EC%82%AC_%EB%B6%84%EB%A6%AC)

모듈형 개발은 '관심사의 분리(separation of concerns, SoC)' 디자인 원칙을 통해 설계됩니다. 관심사의 분리란 말 그대로 각 모듈은 자신의 목적, 관심사에만 초점을 두고 그 외의 다른 부분의 세부 로직에 대해서는 관여하지 않도록 모듈을 설계하는 방식입니다. 각 모듈은 자신이 관심있는 문제를 해결하기 위해 다른 모듈을 활용할 뿐 타 모듈에서 어떤 로직을 통해 input과 output을 결정하는 지는 전혀 신경쓰지 않습니다. 이는 일반적으로 우리가 외부 라이브러리를 사용할 때와 같습니다. React에서 `useState`를 사용할 때를 생각해보면 우리는 `useState`의 내부에 어떤 로직을 가지고 있는 지 고민하지 않습니다. 다만 `useState`를 사용하는 방식과 어떤 return을 제공하는지만 파악해서 활용할 뿐입니다. SoC를 구현하는 프로그램은 모듈러 프로그램이라고 부릅니다. 모듈러 프로그램은 코드가 단순화되면서 유지보수가 더 쉬워지고, 모듈을 더 자유롭게 재사용할 수 있기에 지향되고 있습니다.

## Layered Architecture

Layered Architecture란 관심사의 분리에 따라 시스템을 유사한 관심사(화면 표시, 비즈니스 로직 수행, DB 작업 등)를 가진 계층(Layer)로 분리하고, 각 Layer는 바로 하위 Layer에만 의존하도록 구성하는 아키텍처 패턴입니다. 어플리케이션의 복잡도에 따라 계층의 개수 및 명칭은 다를 수 있으나 일반적으로는 아래 기능을 가진 4개의 레이어로 구분됩니다.

- Presentation Layer: 사용자에게 input을 받고 정보를 제공하기 위한 화면을 보여주는 것을 주 관심사로 두는 계층입니다. 대표적인 구성요소로는 view와 controller가 있습니다. 사용자의 요청과 응답을 담당합니다.
- Business Layer: 비즈니스 로직을 수행하는 것이 주 관심사입니다. 데이터를 받아서 비즈니스 로직을 수행하고 그 결과를 Presentation Layer에 전달합니다. 데이터를 어디서 어떻게 받아왔고, 화면에 정보를 어떻게 뿌릴지는 고려하지 않습니다.
- Persistence Layer: 데이터 엑세스, 보안, 프레임워크 설정 등 app의 영속성을 위해 상위 레이어를 지원하는 것에 관심이 있습니다.
- Database Layer: 실질적으로 DB가 위치한 계층입니다.

Layered Architecture에서 각 layer는 수직적으로 배치되면 각 레이어는 독립적으로 격리되어 다른 레이어의 내부 로직은 모르는 형태로 만들어집니다. 이렇게 만드는 이유는 특정 레이어의 변경사항이 다른 레이어에 영향을 주지 않도록 하기 위해서입니다.

## Flux Architecture (추가 공부 필요)

> 단방향 데이터 흐름에 대해 공부 필요
> 클라이언트 사이드: 클라이언트-서버 구조에서 클라이언트 쪽에서 행해지는 처리를 말한다. (관련 단어: 서버 사이드)

Flux는 Facebook에서 클라이언트 사이드 웹 어플리케이션을 만들기 위해 사용하는 어플리케이션 아키텍쳐입니다. Action, Dispatcher, Stores, Views로 구성되어있으며 단방향 데이터 흐름을 활용해 view 컴포넌트를 구성하는 React를 보완하는 역할을 합니다.

1. Action : 이벤트/메시지 같은 객체.
2. Dispatcher : Store로 Action을 전달. 메시지 브로커와 유사함.
3. Store : 받은 Action에 따라 상태를 변경. 상태 변경을 알림.
4. View(React 컴포넌트) : 화면에 Store의 상태를 반영.

## External Store

> 하단에 예시 코드가 있습니다.

store에는 state 생성, 초기화, update 및 update 되었음을 알리는 모든 state와 관련된 활동이 들어있습니다. store를 react 외부에 둠으로써 state 관리, 비즈니스 로직, UI를 관심사의 분리에 따라 나누어 모듈화하는 방법을 말합니다.

## useReducer

react에서는 state가 변경되어야지만 화면이 리랜더링됩니다. 따라서 단순히 변수에 새로운 값을 할당했을 경우에는 state가 변경되었다고 인지하지 못하기에 화면이 그려지지 않고, setState 함수와 같이 state를 변경해주는 hook을 사용해야만 변경사항이 화면에 반영됩니다. 그러나 비즈니스 로직이 react 외부 모듈로 작성될 경우 로직에 따른 변경사항은 일반적인 js로 작성되기에 변수에 새로운 값을 할당하는 방식으로 적용됩니다. 따라서 이를 화면에 출력하기 위해서는 forceUpdate가 필요합니다. 과거 class 기반의 react에서는 forceUpdate() 함수가 존재했으나, function 기반의 최신 react에서는 이를 useReducer를 사용하여 대신 구현합니다. setState() 함수 또한 내부 로직에서 useReducer를 활용하여 state를 업데이트 합니다.

## useCallback

예시 코드에서 useForceUpdate()를 서로 다른 페이지에서 호출할 경우 이는 동일한 이름과 로직을 가지더라도 useState에 의해 각각 별개의 객체로 나뉘어 관리됩니다. 하지만 코드의 로직을 보면 이는 함수명 그대로 강제로 state에 변화를 주어 화면을 리랜더링하기 위한 목적의 함수이기에, 서로 다른 객체로 관리될 필요가 없습니다. 이처럼 로직 상 개별 관리가 필요 없거나 혹은 로직 상 여러 페이지에서 공용으로 관리되어야 하는 경우 useCallback을 사용하면 개별 객체를 만드는 것이 아니라 기존 객체를 사용하도록 제어할 수 있습니다. 이를 통해 setState를 사용했더라도 모든 페이지에서 사용된 useForceUpdate()는 동일한 함수임을 보장받을 수 있습니다.

```jsx
// ../hook/useForceUpdate.tsx

// store : state update에 관한 모듈입니다.
import { useState, useCallback } from 'react';

export default function useForceUpdate(){
  const [, setState] = useState({});
  return useCallback(() => setState({}, []));
};


// app.tsx

// Business Logic : 별도의 함수로 빼서 UI와 분리하여 사용할 수 있습니다.
import useForceUpdate from '../hook/useForceUpdate';

const state = {
  count: 0,
};

function increase(){
  state.count += 1;
};

// UI : React 부분입니다.

export default function Counter() {
  const forceUpdate = useForceUpdate();

  const handleClick = () => {
    increase();
    forceUpdate();
  };

  return(
    <div>
      <p>{state.count}</p>
      <button type="button" onClick={handleClick}>increase</button>
    </div>
  );
};
```

---

## 강의를 들으면서 든 생각

회사에서 react로 개발을 하면서 공통으로 사용되는 state를 분리하기 위해 useReducer를 사용했었는데, 이번 강의를 통해 useReducer가 어떤 목적을 가지고 만들어졌으며 어떤 로직을 가지고 있는지 알 수 있었다. 또한 두 component에서 동일한 이름을 가진 useReducer를 각각 선언하고, 한 쪽 컴포넌트에서 값을 update 했을 때 다른 쪽 컴포넌트에서 변경사항이 반영되지 않는 오류가 있었는데 이는 오류가 아니라 당연한 현상임을 알 수 있었다.

## 추가로 공부할 것

클라이언트 사이드, 서버 사이드
메시지 브로커
Redux
