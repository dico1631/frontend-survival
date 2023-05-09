# 5주차 2강__React Testing Library

## 설명

### React Testing Library

React Testing Library란 Behavior Driven Test(행위 주도 테스트)를 위한 테스트 코드를 쉽게 작성할 수 있도록 만들어진 테스팅 라이브러리입니다. Behavior Driven Test(행위 주도 테스트)란 app을 이용하는 사용자의 관점에서 사용자가 실제로 경험하는 상황/화면을 위주로 테스트하는 방법으로, 개발자 관점에서 구현된 코드의 의미를 중시하는 Implementation Driven Test(구현 주도 테스트)의 단점을 보완하기 위해 생긴 테스트 방법론입니다.

#### given - when - then 패턴

given - when - then 패턴은 행위 중심으로 테스트를 표현하는 방법입니다. React Testing Library에서는 given - when - then 패턴을 활용하여 테스트 코드를 작성하게 됩니다.

- given: 테스트 시나리오에서 지정하는 동작을 시행하기 전에 상황에 맞도록 환경을 설정하는 부분입니다. 테스트의 전제 조건이라고 생각하면 됩니다.
- when: 언제, 어떤 동작이 실행될 지에 대해 기술하는 부분입니다.
- then: when에서 기술된 동작이 실행되었을 때 예상되는 결과 및 변경사항에 대해 기술하는 부분입니다. 실제 실행 결과와 then에 기술된 내용을 비교하여 fail / success 결과가 판정됩니다.

#### Mocking

외부 의존성이 높은 코드를 테스트할 때 해당 부분을 임시로 구현해두는 코드입니다. 주로 외부 API 통신을 통해 데이터를 조작하는 코드에서 많이 사용됩니다. CRUD 코드를 매 테스트마다 돌리게 된다면 잘못된 데이터 조작이 일어날 수 있습니다. 따라서 실제 코드를 가지고 테스트하는 것은 위험할 수 있습니다. 이러한 문제를 방지하고자 `jest.mock()`과 같이 mocking을 지원해주는 툴을 활용해 테스트용으로 해당 동작을 기술해둘 수 있습니다.

- 예시 코드

```jsx
import { render, screen } from '@testing-library/react';

import App from './App';

**jest.mock**('./hooks/useFetchProducts', () => () => [
	{
		category: 'Fruits', price: '$1', stocked: true, name: 'Apple',
	},
]);

test('App', () => {
	render(<App />);

	screen.getByText('Apple');
});
```

#### 테스트 코드 작성의 효과

component를 사용하는 테스트 코드를 작성하면서 해당 component에 대해 재점검을 할 수 있습니다. 이 과정에서 범용성이 떨어지는 네이밍이나 로직을 발견하거나, 잘못된 코드를 발견할 수 있습니다.

#### Test fixture

Test fixture를 통해 테스트 코드를 작성하면서 생기는 중복 테스트 코드를 모듈화해서 사용할 수 있습니다.

## 강의를 들으면서 든 생각

유튜브로 js 중급 강의를 들을 당시 react test 강의도 있는 것을 보았을 때는 그게 무엇을 하는 강의인지 몰랐었는데, 이번 강의를 통해 역할과 어떤 방식으로 작성하는 지를 배울 수 있었다. 다만 jest와 동일하게 테스트 코드를 작성해본 적이 없어서 아직 문법에 익숙하지 않아 다시 공부할 필요가 있을 것 같다.

### 추가로 공부할 것

jest 문법
react test library 사용법
