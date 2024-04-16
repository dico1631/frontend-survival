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
