# 1주차 2강_TypeScript

## 키워드

- REPL
- TypeScript
  - Interface vs Type
  - 타입 추론
  - Union Type vs Intersection Type
  - Optional Parameter

## 설명

- REPL

### TypeScript

동적 언어인 js에서의 자유도가 오류의 위험성을 높이기에 이를 방지할 수 있는 방법이 필요했음. 이를 위해 js를 정적 언어로 사용할 수 있도록 만든 것이 ts.

- 정적 언어: 실행 전 컴파일 때 변수의 존재 및 타입 등이 지정되는 언어, 변수 선언 시 타입이 지정되며, 지정된 타입의 값만 할당이 가능, vscode에서 자동완성도 잘 됨
- 동적 언어: 실행 타임에 변수에 대해 파악이 되는 언어. 따라서 변수 선언 시 타입을 지정하지 않아도 되며, 자유롭게 변수에 다른 타입을 할당해도 됨

```typeScript
let human: {
  name: string;
  age: number;
};
```

- Interface vs Type: 타입은 새 프로퍼티를 추가하도록 개방될 수 없는 반면, 인터페이스의 경우 항상 확장될 수 있다.

  - 인터페이스

  ```typeScript
  interface Animal {
  name: string
  }

  interface Bear extends Animal {
  honey: boolean
  }

  const bear = getBear()
  bear.name
  bear.honey
  ```

  - 타입

  ```typeScript
  type Animal = {
  name: string
  }

  type Bear = Animal & {
  honey: Boolean
  }

  const bear = getBear();
  bear.name;
  bear.honey;
  ```

- 타입 추론: 변수 생성 시 타입을 명시적으로 지정하지 않고 값을 할당했을 경우, 알아서 해당 값의 타입으로 변수 타입을 추론해 지정함

    ```typeScript
    // 자동으로 string 타입이 됨
    let str = "홍길동"
    ```

- Union Type: 합집합, | 를 이용하여 여러 타입 중 하나가 가능하도록 변수 타입을 선언함
  - 선택지를 주고, 그 중에서만 선택하도록 매개변수를 제한하거나 할 때 매우 유용하게 쓸 수 있다.

  ```typeScript
  let target: string | number;
  target = '홍길동';
  target = 13;
  ```

- Optional Parameter
  - ? 를 써서 옵션을 줄 수 있음

  ```typeScript
  function greeting(name?: string): string {
  return `Hello, ${name || 'world'}`;
  }
  ```

  - 매개변수 값이 들어오지 않을 때를 대비해, 기본값을 지정하기

  ```typeScript
  function greeting(name: string = 'world'): string {
  return `Hello, ${name}`;
  }
  ```

- Intersection Type: 교집합, &를 사용하여 여러 조건을 줌, 모든 조건을 충족해야 함

  ```typeScript
  type Human = {
  name: string;
  age: number;
  };

  type Creature = {
  hp: number;
  mp: number;
  };

  type Person = Human & Creature;

  let person: Person;

  person = { name: '홍길동', age: 13, hp: 256, mp: 16 };
  ```
