# 4주차 2강_Fetch API & CORS

## 설명

### Fetch API 란

Fetch API는 네트워크 통신을 포함해 자원을 얻기 위해 필요한 내용이 담겨져있는 인터페이스입니다. Express를 통해 HTTP Method를 정의했다면, Fetch API의 `fetch("URL 주소");` 함수를 통해 Method를 실행함으로써 자원에 대해 CRUD 작업을 시행할 수 있습니다.
`fetch()`는 매개변수로 url 주소를 받으며 [`Promise` 객체](#h4_1)를 return합니다. 이 때 `Request`의 성공 여부와 상관없이 `Response` 객체가 얻어집니다. 이 `Response`는 요청에 대한 응답으로 body 속성을 통하여 [`ReadableStream`](#h4_2)를 제공합니다.

#### Promise <a id="h4_1"></a>

Promise는 비동기 작업을 다루기 위한 객체의 일종으로 request에 대해 성공했을 경우와 실패했을 경우 각각의 다음 작업을 정의할 수 있습니다. Promise는 아래 3가지 상태 중 한 가지 상태를 지니고 있습니다.

1. 대기(pending): 이행하지도, 거부하지도 않은 초기 상태.
2. 이행(fulfilled): 연산이 성공적으로 완료됨.
3. 거부(rejected): 연산이 실패함.

이 때 fulfilled일 경우는 `.then()`에 정의된 내용이, rejected일 경우에는 `.catch()`에 정의된 내용이 실행되므로서 비동기 작업에 대해 응답이 올 경우 상태에 따라 적절한 처리를 할 수 있도록 합니다. 마지막으로 `finally()`를 통해 이행이나 거부 여부와 상관없이 무조건 비동기 작업 이후에 실행될 작업도 정의할 수 있습니다.

```jsx
fetch('http://localhost:3000/products');
// → Promise

await fetch('http://localhost:3000/products');
// → Response
```

#### ReableStream <a id="h4_2"></a>

`ReableStream`는 Streams API의 인터페이스로 바이트로 되어있는 데이터와 함께 이를 활용할 수 있는 다양한 함수를 담은 스트림 객체를 생성하고 리턴합니다. `ReableStream.getReader()` 함수를 통해 Reader를 만들고 다른 Reader를 얻을 수 없도록 고정한 뒤 `reader.read()` 함수를 통해 데이터가 끝날 때까지 일정 크기로 나누어 데이터를 가져올 수 있습니다. 또한 `ReableStream` 자체에서 json 형식을 지원해주기에 read한 데이터를 text로 변환 후 바로 json으로 파싱하는 것도 가능합니다.

```jsx
const response = await fetch('http://localhost:3000/products');
// → response.body는 ReadableStream

const reader = response.body.getReader();

const chunk = await reader.read();
// → chunk.value는 Uint8Array 타입.
// → 원래는 chunk.done이 true일 때까지 반복해야 한다.

const body = new TextDecoder().decode(chunk.value);

const data = JSON.parse(body);
```

### CORS 란

CORS는 Cross-Origin Resource Sharing의 약자로 서로 다른 웹 주소를 가진 시스템들이 데이터를 주고 받을 수 있도록 서로 허가와 인증을 할 수 있도록 하는 보안 체제입니다. 안전한 데이터 작업을 위해서는 보안 작업이 필수적입니다. 따라서 브라우저는 기본적으로 리소스를 제공하는 쪽과 요청하는 쪽이 같은 웹 주소(포트까지 포함)를 가지고 있을 때만 데이터의 사용을 허가해주는 Same Origin Policy을 취합니다. 이로 인해 제공하는 쪽과 다른 주소를 가진 쪽에서 데이터를 요청할 경우 state: 200으로 request에 대해서는 이행을 해주나 제공되는 데이터를 확인할 수도 사용할 수도 없게 만들어버립니다. 그러나 대부분의 데이터 교환 활동은 서로 다른 웹 주소에서 이루어집니다. 따라서 이를 허용해주기 위하여 데이터를 제공하는 쪽에서 허가해줄 수 있는 웹 주소를 사전에 지정해두고, 해당 조건에 맞는 곳에서 자원을 요청할 경우에는 허락을 해주는 보안 정책을 펼치고 있습니다. 이러한 보안 정책이 CORS입니다.
따라서 Fetch API로 Express에 정의된 자원을 얻기 위해서는 자원을 제공해주는 쪽에서는 Express로 HTTP Method를 정의를 함과 동시에 자원 사용의 허가를 내어줄 웹 주소를 정의해야 합니다. 이 때 CORS 미들웨어를 활용하면 쉽게 이를 정의할 수 있습니다.

패키지 설치

```Bash
npm i cors
npm i -D @types/cors
```

CORS 미들웨어 사용

```jsx
import express from 'express';
import cors from 'cors';

const app = express();

app.use(cors());
```

## 강의를 들으면서 든 생각

국비지원 학원에서 처음 웹 개발에 대해 배울 때 단순히 데이터 허용을 위해서 CORS라는 것이 있습니다. 정도로 넘어갔었기에 데이터 보안이 왜 중요하고 CORS가 무엇인지에 대해 이해할 생각을 하지 못하고 지나갔었다. 그런데 실무를 하면서 데이터 보안이 왜 중요한지를 느낄 수 있었고, 이번 강의를 통해 당시에 내가 배웠던 기술들에 대한 개념과 필요성에 대해 공부하게 되면서 경험하고 공부했던 내용들이 하나로 얽혀가는 느낌이 들었다. 다만 아직도 명확하게 이해한 것은 아니어서 강의를 다시 듣고 문서를 보면서 공부를 해야 할 것 같다.

### 추가로 공부할 것
