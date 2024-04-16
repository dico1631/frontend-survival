# 1주차 2강\_TypeScript

### REPL

**R**ead-**E**val(evaluation)-**P**rint **L**oop의 약어로 사용자가 특정 코드를 입력하면 그 코드를 평가하고 코드의 실행결과를 출력해주는 것을 반복해주는 환경을 말한다. (ex. node 환경에서 js를 입력하면 바로 결과 확인이 가능함)

### (작성필요)&#x20;

*   Interface vs Type: 타입은 새 프로퍼티를 추가하도록 개방될 수 없는 반면, 인터페이스의 경우 항상 확장될 수 있다.

    * 인터페이스

    ```typescript
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

    * 타입

    ```typescript
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
*
*   Union Type: 합집합, | 를 이용하여 여러 타입 중 하나가 가능하도록 변수 타입을 선언함

    * 선택지를 주고, 그 중에서만 선택하도록 매개변수를 제한하거나 할 때 매우 유용하게 쓸 수 있다.

    ```typescript
    let target: string | number;
    target = '홍길동';
    target = 13;
    ```
*   Optional Parameter

    * ? 를 써서 옵션을 줄 수 있음

    ```typescript
    function greeting(name?: string): string {
    return `Hello, ${name || 'world'}`;
    }
    ```

    * 매개변수 값이 들어오지 않을 때를 대비해, 기본값을 지정하기

    ```typescript
    function greeting(name: string = 'world'): string {
    return `Hello, ${name}`;
    }
    ```
*   Intersection Type: 교집합, &를 사용하여 여러 조건을 줌, 모든 조건을 충족해야 함

    ```typescript
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
