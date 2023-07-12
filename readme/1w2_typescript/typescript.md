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
        private lastName: string,
        public nickName: string,
    ) {}
}

const john = new Player("john", "last", "jimi");

john.firstName // 안됨
```
{% endcode %}

#### abstract Class (추상 클래스)

추상 클래스란? 가장 최상위의 추상적인 개념을 작성하여, 이후 만들 클래스들이 이 내용을 상속받음으로써 공통적인 특징을 가질 수 있도록 하는 클래스입니다. 추상 클래스는 오직 다른 클래스에 상속되어서 사용되며, 이를 통해 인스턴스를 생성할 수 없습니다.

<pre class="language-typescript" data-line-numbers><code class="lang-typescript"><strong>abstract class User{
</strong>    constructor(
        private firstName: string,
        private lastName: string,
        public nickName: string,
    ) {}
}

<strong>class Player extends User{
</strong><strong>}
</strong>
const john = new User("john", "last", "jimi"); // 불가능
const john = new Player("john", "last", "jimi"); // 가능
</code></pre>
