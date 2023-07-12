# TypeScript에서 객체지향

### 객체 지향 프로그래밍이란? (작성하기)

TypeScript에서는 일반적인 js가 지원하지 않는 객체 지향 프로그래밍을 위한 기능들을 지원합니다.

### Class

#### constructor (생성자)

ts에서는 constructor에 변수 타입을 지정하게 되면 this로 값을 넣어주는 코드는 자동으로 만들어집니다. 그리고  access modifier(접근 지정자)인 private, public, protected를 통해 class 내필드와 메소드에 접근할 수 있는 권한을 제한함으로써 안정성을 확보할 수 있습니다.

{% code lineNumbers="true" %}
```typescript
class Player{
    constructor(
        private firstName: string,
        protected lastName: string,
        public nickName: string,
    ) {}
}

const john = new Player("john", "last", "jimi");

john.firstName // 불가능
john.lastName// 불가능
john.nickName// 가능
```
{% endcode %}

* private: 해당 클래스의 인스턴스만 접근 가능, 상속을 받은 자식 클래스에서 접근 불가, 클래스 외부에서 접근 불가
* protected: 해당 클래스의 인스턴스와 상속 받은 자식 클래스도 접근 가능, 클래스 외부에서 접근 불가
* public: 클래스 외부에서도 전부 접근 가능, 제한 없음

#### abstract Class (추상 클래스)

추상 클래스란? 가장 최상위의 추상적인 개념을 작성하여, 이후 만들 클래스들이 이 내용을 상속받음으로써 공통적인 특징을 가질 수 있도록 하는 클래스입니다. 추상 클래스는 오직 다른 클래스에 상속되어서 사용되며, 이를 통해 인스턴스를 생성할 수 없습니다.

<pre class="language-typescript" data-line-numbers><code class="lang-typescript"><strong>abstract class User{
</strong>    constructor(
        private firstName: string,
        protected lastName: string,
        public nickName: string,
    ) {}
}

<strong>class Player extends User{...}
</strong>
const john = new User("john", "last", "jimi"); // 불가능
const john = new Player("john", "last", "jimi"); // 가능
</code></pre>

추상 클래스에서는 추상 메소드도 정의할 수 있습니다. 추상 메소드란 내부 로직이 어떻게 되는 지는 상속 받는 자식 클래스 각각에 맞게 작성할 것이나, 반드시 해당 이름으로 된 메소드를 정의하도록 강제하고 싶을 때 사용합니다.

* 사용법: 함수의 call signiture만 작성하고, 내부 로직은 작성하지 않습니다.

```tsx
abstract class User{
    constructor(
        private firstName: string,
        protected lastName: string,
        public nickName: string,
    ) {}
	abstract callFirstName():void // 추상메소드
	abstract callLastName():void // 추상메소드
	abstract callNickName():void // 추상메소드
	
}

class Player extends User{
	// 반드시 여기서 callFirstName, callLastName, callNickName 를 정의해야 함
	callFirstName(){...}
	callLastName(){...}
	callNickName(){...}
}
```

이 때 예시 코드의 추상메소드 `callFirstName()`, `callLastName()`, `callNickName()` 가 각각 firstName, lastName, nickName를 콘솔에 찍는 함수라면 `callLastName()`, `callNickName()`는 구현할 수 있으나 `callFirstName()`는 firstName이 `private`이기에 구현될 수 없습니다. private 접근 지정자인 필드나 메소드는 자식 클래스에서도 접근할 수 없기 때문입니다. 따라서 추상 클래스의 경우 대부분의 항목을 `protected`로 지정합니다.

{% code title="완성 코드" %}
```tsx
abstract class User{
    constructor(
        protected firstName: string,
        protected lastName: string,
        protected nickName: string,
    ) {}
	abstract callFirstName():void // 추상메소드
	abstract callLastName():void // 추상메소드
	abstract callNickName():void // 추상메소드
	
}

class Player extends User{
	// 반드시 여기서 callFirstName, callLastName, callNickName 를 정의해야 함
	callFirstName(){console.log(firstName)}
	callLastName(){console.log(lastName)}
	callNickName(){console.log(nickName)}
}

const john = new Player("john", "last", "jimi");

john.callFirstName //"john"
john.callLastName //"last"
john.callNickName //"jimi"쇼
```
{% endcode %}
