# 1주차 2강\_TypeScript

### REPL (작성필요)&#x20;

### TypeScript란?

#### TypeScript란?

javascript는 자유도가  높다는 장점이 있지만, 이러한 점이 역으로 개발자가 오류를 내더라도 사전에 알기 어렵다는 문제를 일으켰습니다. java나 c와 같이 규칙이 엄격한 언어의 경우 개발을 진행하면서 에디터가 오류를 알려줄 수 있지만, javascript에서는 모든 경우가 문법적으로 가능하기에 실제로 실행해보지 않으면 오류가 날 지 여부를 알 수 없었습니다. 따라서 이러한 문제를 해결하고자 javascipt 기반이나 strict 한 언어와 같이 변수의 type을 지정해야 하는 언어를 만들었고, 이것이 typescript입니다.

* javascript = 정적 언어
  * 실행 전 컴파일 때 변수의 존재 및 타입 등이 지정되는 언어, 변수 선언 시 타입이 지정되며, 지정된 타입의 값만 할당이 가능, vscode에서 자동완성도 잘 됨
* typescript = 동적 언어
  * 실행 타임에 변수에 대해 파악이 되는 언어. 따라서 변수 선언 시 타입을 지정하지 않아도 되며, 자유롭게 변수에 다른 타입을 할당해도 됨

```typescript
let human: {
  name: string;
  age: number;
};
```

#### typeScript 기초

1. Explicit Types(명시적 타입) VS implicit Types(암시적 타입)

변수의 type를 지정하는 방법은 명시적으로 작성하는 Explicit Types(명시적 타입)과 할당된 값에 따라 자동으로 지정되는 implicit Types(암시적 타입)이 있습니다. implicit Types으로 타입이 자동으로 지정되는 것을 더 권장합니다. 따라서 implicit Types와 Explicit Types 모두 사용이 가능할 경우에는 타입을 쓰지 않습니다.

* implicit Types: 변수 생성 시 타입을 명시적으로 지정하지 않고 값을 할당했을 경우, 알아서 해당 값의 타입으로 변수 타입을 추론해 지정함  (타입 추론)

```typescript
//string 타입
let str = "홍길동"

//number 타입
let num = 1

//boolean 타입
let isOpened = true
```

* Explicit Types: 변수 생성 시 해당 변수에 들어갈 값의 타입을 코드에 직접 작성함
  * 타입은 대상 뒤에 `: 타입` 을 넣어서 명시합니다.

```typescript
let str : string = "홍길동"
let num : number = 1
let nums : number[] = [1,2,3]
let isOpened : boolean = true
```



2. optional parameter

* : 앞에 ?를 넣음으로써 해당 항목을 필수가 아닌 선택적 항목으로 만듭니다.
  * 아래 코드에서 name은 필수이고, age는 있어도 되고 없을 수도 있습니다.

<pre class="language-typescript"><code class="lang-typescript"><strong>const player : {
</strong>    name: string,
    age?: number,
} = {
    name: '민정',
    age: 29,
}
</code></pre>



3. Alias (별칭)

객체의 type을 모듈화하여 사용할 수 있게 하는 방법입니다.

<pre class="language-typescript"><code class="lang-typescript">type PlayerProps = {
    name: string,
    age?: number,
}
<strong>
</strong><strong>const player : PlayerProps  = {
</strong>    name: '민정',
    age: 29,
}

const player : PlayerProps  = {
    name: '정은,
    age: 34,
}
</code></pre>



4. 함수의 type

함수는 매개변수와 return 타입을 지정할 수 있습니다.

```typescript
type PlayerProps = {
    name: string,
    age?: number,
}

function playerMaker(name : string) : PlayerProps {
    return {
        // name: name을 줄여서 하나로 작성 
        name
    }
}
```

5. readonly 속성&#x20;

속성 지정 전에 readonly 예약어를 붙일 경우 해당 변수는 값을 변경할 수 없이 read만 가능한 변수가 됩니다.

```typescript
const numbers: readonly number[] = [1, 2, 3]
numbers.push(4) // readonly 이기에 오류
```

5. tuple (튜플)

배열 내 값들이 순서에 따라 속성이 고정되어있는 배열입니다.

```typescript
const player: [string, number, boolean] = ['민정', 29, true]

// readonly 와 합쳐서 사용도 가능, 이 경우 배열 내 값을 변경할 수 없음 
const player2: readonly [string, number, boolean] = ['민정', 29, true]
```

5. any

any는 typeScript의 보호에서 완전히 벗어나게 하는 예약어입니다. any를 쓰면 javascipt와 완전히 동일하게 어떤 값이 들어가도 오류가 나지 않습니다.

<pre class="language-typescript"><code class="lang-typescript"><strong>// 전부 오류가 나지 않습니다 .
</strong><strong>let a : any = "안녕";
</strong>a = 1;
a = false;

let b: any[] = [1, 2, 'a']
a + b
</code></pre>

6. unknown

어떤 타입이 들어올 경우 사용합니다 . 주로 api와 같이 외부와 소통을 할 경우 사용되며, 이 경우 typeof로 타입을 체크하지 않고 사용하려고 하면 오류 메세지를 띄웁니다.

```typescript
let a: unknown;

let b = a + 1 // 오류

if(typeof a === 'number'){let b = a + 1} // 정상 
```

7. void

함수가 return을 하지 않을 경우에는 void type이라고 합니다. void는 명시적으로 작성하지 않아도 타입 추론을 통해 추론됩니다. 이는 해당 함수를 사용할 때 return 값을 받으려고 할 경우 오류를 알려줍니다.

```typescript
function hello(){
    console.log("안녕");
}
```

8. never

never은 함수가 **절대 return을 하지 않는다**는 의미입니다. 대표적으로 오류를 throw하는 경우가 있습니다 . 또한 조건문에서 가능한 모든 경우를 거친 else에서 사용되는 변수는 type이 never입니다.

```typescript
function hello():never{
    throw new Error("xxx");
}

function hello(name:string|number){
    if(typeof name === "string"){} // 여기서 name은 string 타입 
    else if(typeof name === "number"){} // 여기서 name은 number타입 
    else{} // 여기서 name은 never 타입 
}
```



#### TypeScript에서 함수&#x20;

1. call signiture

함수를 호출할 떄 argument와 return 값의 type을 알 수 있도록 하는 방법으로 함수를 사용할 사람에게 가이드를 줄 수 있습니다. parameter 각각의 type을 지정하는 대신, type 예약어를 통해 타입을 묶어서 정의합니다.&#x20;

```typescript
type AddProps = (a: number, b: number) => number; //return을 하지 않을 경우에는 void

const add:AddProps = (a, b) => a + b;
```

2. overloading

typescript에서 overloading이란 call signiture를 여러 개 두어 다양한 type과 상황을 커버할 수 있도록 type을 지정하는 것입니다.

* 여러 type의 argument가 가능하도록 만듦&#x20;

<pre class="language-typescript"><code class="lang-typescript">type ConfigProps = {
    path: string,
    state: object
}
<strong>
</strong><strong>type PushProps = {
</strong>    (path: string): void
    (config: ConfigProps): void
}

const puch: PushProps = (config) => {
    if(typeof config === "string" ) {console.log(config)}
    else{console.log(config.path)}
}

// 아래 둘 다 가능 
Router.push("/home")

Router.push({
    path: "/home",
    state: false
})
</code></pre>

* argument의 일부를 optional로 만듦&#x20;

```typescript
type AddProps = {
    (a: number, b: number): number
    (a: number, b: number, c: number): number
}

// c는 optional이기에 ?: 표기 필요 
const add:AddProps = (a, b, c?: number) => {
    if(c){return a+b+c}
    return a+b;
}
```

3. generic type

typeScript는 generic type을 통해 Polymorphism(다형성)을 표현할 수 있도록 해줍니다. Polymorphism이란 many의 의미를 가진 poly와 structure의 의미를 가진 morphism의 합성어로, 말 그대로 다양한 구조를 가진 형태를 말합니다. 그리고 typeScript에서의 다양한 형태란, 다양한 type을 의미합니다.

즉, concrete type 중 어떤 타입이 어떤 조합으로 들어올 지 모르는 상황에서는 type을 명확하게 정의하는 대신에 generic type을 사용하여 type을 지정합니다. generic type은 typeScript의 타입 추론을 활용하여, 실제 사용할 때 들어오는 argument에 따라 해당 함수의 parameter type과 return type을 통해 타입을 추론하게 하는 방법을 말합니다.

예시를 통해 사용방법과 정의를 알아봅시다.



아래 코드처럼 superPrint에 number\[], boolean\[], string\[] 뿐만 아니라 이들이 섞여있는 배열까지도 무엇이든 들어올 수 있는 경우에 모든 case를 명시적으로 작성하는 것은 불가능합니다.

```typescript
// 이처럼 모든 case를 다 적어줘야 함, 이는 불가능
type SuperPrintProps = {
	(arr: number[]): number
	(arr: string[]): string
	(arr: boolean[]): boolean
	(arr: (number|boolean)[]): (number|boolean)
	(arr: (number|boolean|string)[]): (number|boolean|string)
}

const superPrint: SuperPrintProps = (arr) => arr[0];

const a = superPrint([1,2,3,4]);
const b = superPrint(["a","b","c"]);
const c = superPrint([true, false]);
const d = superPrint([1,2,true]);
const e = superPrint([1,2,true,"a"]);
```



따라서 이런 경우 타입 추론을 하여 상황마다 타입을 정할 수 있도록 generic type을 씁니다. 여기서 T는 대명사로 어떤 단어도 사용할 수 있습니다. 이 T 자리는 사용할 당시 들어오는 argument에 따라 그 타입이 추론되어 들어갈 것입니다.

```typescript
// 앞에 <T> 를 통해 T가 generic type을 사용하는 대명사라는 것을 암시
type SuperPrintProps = <T>(arr: T[]) => T;

const superPrint: SuperPrintProps = (arr) => arr[0];
t
const a = superPrint([1,2,3,4]);
const b = superPrint(["a","b","c"]);
const c = superPrint([true, false]);
const d = superPrint([1,2,true]);
const e = superPrint([1,2,true,"a"]);
```



generic type을 여러 개 사용하고 싶을 땐 <>에 , 로 여러 개를 넣으면 됩니다.

```typescript
// <>에 , 로 여러 개의 generic type을 넣음
type SuperPrintProps = <T, M>(arr: T[], b: M) => T;

const superPrint: SuperPrintProps = (arr, b) => {
	console.log(b);
	return arr[0];
};

const a = superPrint([1,2,3,4], "b");
const b = superPrint(["a","b","c"], 1);
const c = superPrint([true, false], true);
const d = superPrint([1,2,true], "name");
const e = superPrint([1,2,true,"a"], false);
```







:thumbsup::thumbsup::thumbsup::thumbsup::thumbsup::thumbsup::thumbsup::thumbsup::thumbsup::thumbsup::thumbsup:

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
