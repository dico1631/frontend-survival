# TypeScript에서 함수

## call signiture

함수를 호출할 떄 argument와 return 값의 type을 알 수 있도록 하는 방법으로 함수를 사용할 사람에게 가이드를 줄 수 있습니다. parameter 각각의 type을 지정하는 대신, type 예약어를 통해 타입을 묶어서 정의합니다.&#x20;

<pre class="language-typescript"><code class="lang-typescript">type AddProps = (a: number, b: number) => number; //return을 하지 않을 경우에는 void
<strong>
</strong>const add:AddProps = (a, b) => a + b;
</code></pre>

## overloading

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

## generic type

> generic type은 대문자로 시작합니다 .

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

일반적으로 generic type은 외부 라이브러리를 사용하거나 , 혹은 만들 때 사용됩니다. 또한 객체의 일부에 type 자유도를 줄 때 generic type를 사용할 수 있습니다. 그러면 객체를 생성할 때 들어가는 데이터를 통해 타입 유추를 하여 type이 결정됩니다. 이러한 속성을 이용해서 더 큰 type을 만들고, 이에 대한 세분화 된 type은 따로 선언하여 type들을 재사용 할 수 있는 모듈로 만들 수 있습니다.

```typescript
type PlayerProps<E> = {
	name: string,
	extraInfo: E //어떤 type의 내용이든 올 수 있음
}

// extraInfo은 string property를 가진 객체

// 2번
type NicoExtraProps = {hobby: string}

const nico: PlayerProps<NicoExtraProps> = { //const nico: PlayerProps<{hobby: string}>
	name: "nico",
	extraInfo: {
		hobby: "swimming",
	}
}

// 3번: extraInfo은 null
const mickey: PlayerProps<null> = {
	name: "nico",
	extraInfo: null,
}
```

generic type은 typescript에서 제공하는 다양한 내장 객체에서도 사용됩니다. 대표적으로 `Array` 객체의 경우 배열 안에 어떤 원소든 들어갈 수 있도록 하기 위해 `Array<T>`로 되어있습니다. 따라서`Array<number>`은 `number[]`와 같습니다.
