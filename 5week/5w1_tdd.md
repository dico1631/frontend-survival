# 5주차 1강_TDD

## 설명

### TDD란

TDD는 테스트 코드를 먼저 작성하여 인터페이스와 스펙을 먼저 정의한 뒤, 작성된 테스트 코드를 기준으로 로직을 구현하는 개발 방식입니다. 자동화 테스트 코드를 먼저 작성 후 구현, 리펙토링을 통해 코드를 올바르게 수정하는 순서로 진행됩니다. 이는 프로그래머스와 같이 알고리즘 문제를 제공하는 사이트로 알고리즘 문제를 풀어보는 것과 같은 프로세스입니다. 알고리즘 문제를 공부할 때 우리는 아래와 같은 순서를 거칩니다.

1. 우선 사이트에는 input과 예상되는 올바른 결과로 이루어진 테스트 시나리오가 준비되어 있습니다.
2. 우리는 주어진 함수의 input을 활용해 로직을 구현하고, 제공되는 테스트 시나리오를 통과할 수 있도록 코드를 구현합니다.
3. 그리고 모든 테스트를 통과한 뒤에도 내 코드를 재점검하고, 다른 사람의 코드를 보면서 더 나은 방법이 있는지 확인합니다.

TDD는 이 순서와 동일한 사이클을 돌면서 점점 더 견고한 코드를 구성해나가는 방법입니다. TDD의 cycle은 아래와 같습니다.

1. red: 우선 인터페이스와 스펙에 집중하여 테스트 코드를 작성합니다. 이 테스트 코드들은 당연히 실패하는 테스트 코드입니다.
2. green: red에서 작성된 테스트를 통과할 수 있는 코드를 작성합니다. 여기서는 올바르고 퀄리티가 좋은 방법보다 빠른 속도가 중요합니다. 우선 테스트를 통과할 수 있도록 만듦니다.
3. refactor: green에서 만든 코드를 탐구하고 수정하면서 퀄리티를 높여가는 과정입니다. 실행 동작(스펙)은 유지하면서 내부 실행 로직을 수정합니다.

TDD의 2단계에 나와있는 것처럼 TDD는 한 번에 완벽한 코드를 짜려고 노력하지 않습니다. 대신 반복적인 수정 작업을 거칩니다. 따라서 빠르게 사이클을 돌려서 반복적으로 refactor를 할 수 있도록 하는 것이 제일 중요합니다. 일반적으로 10분 안에 한 사이클을 돌릴 수 있어야 하며, 만일 TDD 사용에 숙련되어 있는 사용자가 사이클이 오래 걸릴 경우에는 그 원인을 찾아 TDD 작업을 개선해야 합니다. (ex: green 단계에 시간이 오래 결릴 경우 red에서 작성된 코드의 단위가 너무 클 수도 있습니다.)

#### TDD의 장점

테스트를 먼저 작성하고 구현을 하면, 테스트 하기 쉬운 코드를 작성하게 됩니다. 그리고 이렇게 개발을 하다보면 각 모듈의 역할이 단순하고 명확해집니다. 즉 자연히 모듈의 크기가 줄어들게 되고, 최대한 단순하게 모듈을 얽으면서 결국 유지보수와 확장이 쉬운 코드를 작성하게 됩니다.

- 각 모듈의 역할이 단순해지고 명확해짐
- 프로젝트의 유지보수와 확장이 쉬움
- 수동 테스트에서 시간을 단축할 수 있음
- 놓칠 수 있는 것들을 반복 테스트하기 좋음
- 프로젝트의 품질을 높이고, 효율적인 테스트 경험과 사용자의 입장을 고려한 개발을 진행할 수 있도록 해줌
- 실제 사용자의 실행 환경과 거의 동일한 환경에서 테스트를 진행하기 때문에, 실제 상황에서 발생할 수 있는 에러를 사전에 발견할 수 있음

### Jest

TDD를 위해서는 테스트 코드를 작성하고 테스트를 작동시켜줄 테스팅 도구가 필요합니다. Jest는 페이스북에 만든 테스팅 도구로 테스트에 필요한 대부분의 기능을 가지고 있습니다.

- 예시 코드

```jsx
// test: 테스트에 대한 간단한 설명과 테스트 코드를 넣는다.
test('테스트 설명', () => {
  //expect: 실행 항목을 작성
  //toBe: matchar 함수를 통해 예상 결과를 작성, matchar로 사용할 수 있는 다양한 함수를 제공함
  expect(fn.makeUser('mike', 30)).toBe({
    name: 'mike',
    age: 30
  });

  expect(fn.makeUser('mike', 30)).toBeEqual({
    name: 'mike',
    age: 30
  });
});
```

- matchar

expect의 실행 결과가 예상되는 결과와 일치하는지 매칭해주기 위한 함수
[matchar 함수 전체 목록](https://mulder21c.github.io/jest/docs/en/next/expect)

#### Describe - Context - It 패턴

테스트 케이스를 정의하는 방법은 크게 아래 2가지로 나뉩니다. 이때 2번째 방식이 Describe - Context - It 패턴을 활용한 방식입니다.

1. test 함수로 개별 테스트를 나열하는 방식.
2. [BDD 스타일](#bdd)로 주체-행위 중심으로 테스트를 조직화하는 방식.

테스트 대상과 다양한 상황 및 그 결과를 기술하여 다양한 상황에 대해 알기 쉽게 테스트 할 수 있도록 하는 테스트 코드 작성법입니다. 테스트 대상을 Describe한 뒤 대상에서 발행할 수 있는 case들을 Context에 넣고, it에 해당 context일 경우 예측되는 결과를 기술합니다.

```jsx
function add(...number: number[]){
  return (number[0] ?? 0 ) + (number[1] ?? 0 ) + (number[2] ?? 0 );
}

// test 함수로 개별 테스트를 나열
test('add', () => {
  expect(add(1, 2)).toBe(3);
});

// BDD 스타일: Describe - Context - It 패턴
const context = describe;

describe('add', () => {
  context('with no argument', () => {
    expect(add()).toBe(0);
  });
  context('with 1 argument', () => {
    expect(add(2)).toBe(2);
  });
  context('with 2 argument', () => {
    expect(add(1, 2)).toBe(3);
  });
  context('with 3 argument', () => {
    expect(add(1, 2, 3)).toBe(6);
  });
});
```

##### BDD란? <a id="bdd"></a>

BDD란 Behavior Driven Development의 약자로 사용자의 행위 중심으로 테스트 시나리오를 작성합니다. 사용자 중심으로 진행되기에 요구사항 정의서, 기능정의서, 기획서와 같이 개발 시작 전의 산출물을 통해 테스트 시나리오가 작성됩니다. 덕분에 이후 누락된 기획 항목이 발생되었을 때도 시나리오 개선 및 로직 구현이 더 용이하다는 장점이 있습니다.

### 단위테스트(Unit Test) vs 통합테스트(Integration Test) vs 인수테스트(Acceptance Test)

테스트는 대상의 단위 크기에 따라 단위테스트, 통합테스트, 인수테스트로 불립니다. TDD로 테스트하는 코드는 단위테스트에 해당합니다.

- **단위테스트**란 프로그램을 클래스, 메소드 등 테스트 가능한 가장 작은 단위로 쪼개어 해당 단위의 로직이 잘 작동하는 지 알기 위한 테스트입니다. 따라서 개별 단위가 잘 기능하는 지를 파악하기 위해서 제일 처음 시행하는 테스트입니다.

- **통합테스트**란 단위테스트보다 더 큰 동작을 하는 여러 모듈들이 서로 잘 협력하는 지 알기 위한 테스트입니다. DB 접근, 다양한 상황에서 코드가 작동되는 지와 더불어 외부 라이브러리와 같이 개발자가 통제할 수 없는 상황까지 함께 검증합니다.

- **인수테스트**란 사용자 스토리(시나리오)에 맞춰 수행하는 테스트입니다.

## 강의를 들으면서 든 생각

알고리즘 문제를 풀 때 10개 정도의 input과 답변이 있고, 이를 통과할 수 있는 코드를 짜기 위해 노력하는데, 이 점이 TDD와 비슷하다는 생각이 들었다. 다만 TDD로 개발하기 위해서는 답변 코드를 내가 짜야한다는 것이 달랐다. TDD를 통해 견고한 프로그램을 만들 수 있다는 것에 약간의 감이 오기는 하나, 테스트 코드 작성 또한 개발이기에 시간과 비용이 들고 사람의 실수가 발생할 수 있다는 점에서 같은 것이 아닌가 하는 생각도 들었다. TDD와 BDD의 장단점과 JEST의 장점은 실제 테스트를 여러 번 적용해보아야 알 수 있을 것 같다.

### 추가로 공부할 것

시그니처, 인터페이스(시그니처 모음), 스펙
