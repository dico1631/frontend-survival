# 6주차 2강\_TSyringe

## TSyringe

TSyringe은 TypeScript용 DI 도구(DI container)로, 이를 통해 External Store을 관리할 수 있습니다. React 입장에서 Tsyringe로 만들어진 store는 전역 범위를 가지기에 React 외부에서 state를 관리하고, 이를 React 모든 부분에서 사용할 수 있게 됩니다. 또한 state 관리를 외부에서 진행하기에 Prop(state 변수 및 update 함수를 상위 컴포넌트에서 하위 컴포넌트로 전달할 때 사용되는 값)를 chain 형식으로 전달할 필요가 없습니다. 따라서 Component chain이 깊어질수록 더 많은 props를 더 깊이(여러차례 반복하여) 전달해야 하는 React의 Prop Drilling 문제를 해결할 수 있습니다.

### 의존성 주입(DI: Dependency Injection)

TSyringe은 TypeScript용 DI 도구입니다. 그렇다면 DI는 무엇일까요?

#### 의존성

> 의존대상 B가 변하면, 그것이 A에 영향을 미친다. -이일민, 토비의 스프링 3.1, 에이콘(2012), p113

의존성이란 말 그대로 의존적인 관계를 말하는 것으로 A가 B를 이용하고, A는 B가 없이 작동할 수 없을 때 A는 B에 의존한다고 합니다. 여기서 A, B는 함수, 클래스, 객체 등 하나의 묶음 단위입니다.

<details>

<summary>의존 관계를 보여주는 예시 코드</summary>

```jsx
const sandwich = () => {
  const softBread = softBread();
  const spicyHam = spicyHam();
  const lettuce = lettuce();
  const mozzarella = mozzarella();
  // softBread, spicyHam, lettuce, mozzarella로 샌드위치를 만드는 로직 구현
  return sandwich;
};

const softBread = () => (softBread);
const spicyHam = () => (spicyHam);
const lettuce = () => (lettuce);
const mozzarella = () => (mozzarella);

// 시작
main(){
  sandwich();
};
```

이 코드 로직에 따르면 우리는 반드시 softBread, spicyHam, lettuce, mozzarella로 이루어진 샌드위치만을 먹을 수 있습니다. 다른 빵, 햄, 채소, 치즈 종류는 넣을 수 없습니다. 만일 매운 것을 먹지 못해 spicyHam을 sweetHam으로 바꾸고자 spicyHam 코드를 바꾸거나 새로운 sweetHam용 함수를 만들면 어떻게 될까요? 우선 spicyHam 코드를 바꿔서 sweetHam를 반환하게 한다면, 함수명 또한 sweetHam로 바꿔야 합니다. (바꾸지 않을 경우 함수 로직과 함수명이 다르게 되기에 이 경우는 고려하지 않습니다.) 그러면 이를 사용하는 sandwich의 코드의 spicyHam 호출 부분과 이를 사용하는 모든 부분 또한 변경해야 합니다. 그렇지 않으면 함수를 호출할 때 오류가 나기 떄문입니다. 새로운 sweetHam용 함수를 만드는 경우에도 sandwich 코드를 변경해야만 우리가 원하는 sweetHam을 가진 sandwich를 먹을 수 있습니다. 즉, sandwich에 속한 재료 함수가 변경될 경우 원하는 결과를 위해서는 반드시 sandwich 코드도 같이 변경되어야 합니다. sandwich는 각 재료 함수에 의존하고 있습니다.

</details>

* 의존관계의 문제점

의존적인 관계일 경우 한 모듈을 변경하게 되면 이를 사용하는 모든 항목을 다 변경해야 합니다. 그렇기에 의존 대상 변화의 파급력이 너무 커서 결국 개발자는 코드의 문제가 발견되거나 더 범용성 있도록 만들고자 하여도 수정하기가 부담스러워집니다. 결국 재사용을 하기 위해 코드를 수정하기보다는 비슷한 다른 코드를 새로 만드는 선택을 할 수 밖에 없습니다. 또한 테스트를 진행할 때도 모듈들이 서로 연관이 있기 때문에 독립적으로 테스트를 할 수 없게 됩니다.

#### 의존성 주입(DI)

의존성 주입은 이러한 의존성을 해소하여 코드의 자유도를 높이고자 고안된 개발 테크닉로, 생성자나 setter 함수, interface를 활용해 제 3자의 역할을 주입함으로써 의존성을 약화시킵니다.

<details>

<summary>의존성 주입을 하는 3가지 방법</summary>

1. 생성자 주입

```jsx
function Ingredient(bread, ham, vegetable, cheese){
  const this.bread = bread;
  const this.ham = ham;
  const this.vegetable = vegetable;
  const this.cheese = cheese;
};

const sandwich = (ingredient) => {
  // ingredient.bread , ingredient.ham , ingredient.vegetable , ingredient.cheese로 샌드위치를 만드는 로직
  return sandwich;
};

// 시작
main(){
  const ingredient = new Ingredient(softBread, spicyHam, lettuce, mozzarella);
  const ingredient = new Ingredient(softBread, sweetHam, lettuce, mozzarella);

  sandwich(ingredient); // spicyHam sandwich
  sandwich(ingredient); // sweetHam sandwich
};
```

위 코드에서는 Ingredient 생성자를 통해 재료를 받았습니다. 그리고 sandwich 함수는 Ingredient 생성자를 통해 만들어진 객체를 전달받아 이 재료들로 샌드위치를 만듭니다. 그 덕분에 재료가 달라지더라도 new로 생성자를 생성할 때의 input만 다를 뿐 sandwich 함수 내부 로직은 바뀌지 않았습니다. 이처럼 다른 함수에 의존하고 있던 부분을 생성자로 분리하면 의존 관계였던 클라이언트(sandwich 함수)는 더 자유로워질 수 있습니다. 만일 여기에 spread 연산자와 map과 같은 함수까지 더해진다면 재료의 개수가 더 많거나 적더라도 우리는 코드 변화없이 더 다양한 샌드위치를 만들 수 있게 됩니다.

2. Setter 주입

```jsx
const Ingredient = {
  let bread: 'bread',
  let ham: 'ham',
  let vegetable: 'vegetable',
  let chees: 'chees',

  get bread(){return Ingredient.bread};
  set bread(bread){Ingredient.bread = bread};

  get ham(){return Ingredient.ham};
  set ham(ham){Ingredient.ham = ham};

  get vegetable(){return Ingredient.vegetable};
  set vegetable(vegetable){Ingredient.vegetable = vegetable};

  get cheese(){return Ingredient.cheese};
  set cheese(cheese){Ingredient.cheese = cheese};

};

const sandwich = (ingredient) => {
  // ingredient.bread , ingredient.ham , ingredient.vegetable , ingredient.cheese로 샌드위치를 만드는 로직
  return sandwich;
};

// 시작
main(){
  Ingredient.bread = softBread;
  Ingredient.ham = spicyHam;
  Ingredient.vegetable = lettuce;
  Ingredient.cheese = mozzarella;

  sandwich(ingredient); // spicyHam sandwich

  Ingredient.ham = sweetHam;
  sandwich(ingredient); // sweetHam sandwich
};
```

ES6 최신 자바스크립트부터는 Getter와 Setter를 간단하게 정의할 수 있는 문법이 별도로 추가되었습니다. getter와 setter는 데이터에 직접적으로 접근하지 못하게 하여 보안을 높여주는 방법입니다. 이 방법 또한 생성자 주입과 동일하게 다른 재료를 넣고 싶으면 setter로 재료만 변경하면 다른 코드를 수정할 필요는 없습니다.

3. Interface 주입

```java
interface Animal { public abstract void cry(); }

class Cat implements Animal {
  public void cry() {
    System.out.println("냐옹냐옹!");
  }
}

class Dog implements Animal {
  public void cry() {
    System.out.println("멍멍!");
  }
}
```

인터페이스는 변수나 함수, 클래스 등이 만족해야 하는 최소한의 규격을 정하게 해주는 도구입니다. JAVA에서는 이를 통해 최소한의 내용만 추상적으로 적어두고, 상세 내용은 각 클래스에서 사용하게 함으로써 의존 관계를 약화시킵니다. 코드를 보면 Animal에서 cry를 구현하기 때문에 Cat과 Dog는 서로의 울음소리가 달라져도 전혀 영향을 받지 않습니다. 또한 interface가 있기 때문에 새로운 동물이 나오더라도 다른 코드에 영향을 주지 않고 cry를 사용할 수 있습니다.

</details>

### TSyringe를 활용한 External Store 구현 예시

TSyringe에는 DI를 위한 다양한 기능이 존재합니다. 전체 기능을 알고 싶다면 아래 링크를 참고하세요. [TSyringe github](https://github.com/microsoft/tsyringe#tsyringe)

#### reflect-metadata

TSyringe를 쓰기 위해서는 모든 코드가 시작하는 곳에 reflect-metadata를 import 해야만 합니다. main.tsx와 같이 React가 시작하는 최상단과 setupTest.ts와 같이 test 코드가 시작하는 최상단에 import 합니다.

```jsx
import 'reflect-metadata';
```

#### sington (싱글톤)

객체의 경우 new로 생성을 할 때마다 새로운 객체가 생성되어 메모리에 저장됩니다. 하지만 싱글톤으로 객체를 구성할 경우 최초의 new에서만 객체가 생성되고, 그 이후의 new를 할 땐 이전에 생성된 객체를 불러옵니다. new를 할 때마다 대상이 있는 지 확인하고, null일 경우 객체를 생성하고 객체가 존재할 경우 이를 가져오는 방식으로 설계되어 있습니다. 이를 통해 어떤 상황에서 new를 해도 모두 동일한 대상을 가르킨다는 것을 보장할 수 있습니다. React의 state 또한 객체와 같이 생성 코드(ex. useState)가 사용될 때마다 새로운 state가 만들어져서 메모리에 저장됩니다. 그러나 External Store을 사용하는 경우 state를 외부에 두고 공용으로 사용하려는 목적을 가지고 있습니다. 따라서 프로젝트의 어떤 부분에서 이 것이 호출되어도 모두 같은 대상을 가르켜야만 합니다. 이를 위해서 External Store 또한 싱글톤 패턴으로 만들어져야 합니다. TSyringe의 `@singleton()`를 통해 쉽게 구현될 수 있습니다.

* External Store 부분

```jsx
import { singleton } from 'tsyringe';

@singleton() // 이 부분을 쓰면 하단 클래스를 싱글톤으로 만들어 줌
class CounterStore {
  // …(중략)...
}
```

* External Store를 사용하는 부분

```jsx
import { container } from 'tsyringe';
import { CounterStore } from '../stores/counterStore.tsx';

// tsyringe는 DI container(사용할 때 container를 사용), container 코드로 호출해서 쓰면 알아서 의존성 주입 등 필요한 것을 해 줌.
const counterStore = container.resolve(CounterStore);
```

<details>

<summary>멀티스레드 환경에서 싱글톤의 한계</summary>

멀티스레드 환경에서는 여러 new가 거의 동시에 호출될 수 있습니다. 이 경우 한 번에 대상이 있는 지 확인하게 되면서 모두 null이라고 인식하여 객체를 생성하게 되는 경우도 발생합니다. 따라서 멀티스레드 환경에서는 싱글톤이 무조건 같은 객체를 반환하리라는 보장을 하기 어렵습니다.

</details>

## 강의를 들으면서 든 생각

이번 강의는 DI와 IoC에 대해 공부하느라 데브노트를 정리하는데 오래 걸렸다. 이번에 처음 들은 단어는 아니었으나, 언제나 조금 공부하다가 어렵다고 포기했던 단어였는데, 다시 등장하는 것을 보고 이번엔 무조건 잡고 가야겠다는 생각이 들어서 유독 시간을 더 많이 쏟은 것도 있었다. JAVA를 배웠었고 java로 여러 프로젝트를 해보았었지만 그때도 그 단어를 정확하게 이해하지는 못하고 대충 알겠다는 느낌만 가지고 넘어갔었다. 그리고 이젠 java가 흐릿해져서 더 이해하기 어려웠다. 그래도 시간을 쏟은 덕분에 DI가 무엇이고, 왜 사용되는지, DI를 하기 위한 방법은 무엇이고 코드로 어떻게 표현하는 지까지 알게 되어서 만족스러운 시간이었다. 다만 내가 작성한 내용이 DI가 정확히 맞는지에 아직 확신할 수 없다. 특히 대부분의 예시 코드가 java로 되어있어 이를 해석하고 js로 바꿔보았는데 모든 것을 '이런 것 같은데?' 느낌으로 작성한 것이라 나중에 더 많이 공부를 하게 되면 작성한 내용을 보고 재해석을 해보아야 할 것 같다.

## 추가로 공부할 것

context(React에서 External Store 관리를 할 수 있게 해주는 것) 제어의 역전(IoC: Inversion of Control)
