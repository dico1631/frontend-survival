# 4주차 1강\_Express

## 설명

### Express 란

Express는 node.js를 사용하여 쉽게 서버를 구축하고 API를 만들 수 있도록 하는 웹 애플리케이션 프레임워크입니다. 아래 코드 예시와 같이 Express를 통해 HTTP Method를 쉽게 정의하고 사용할 수 있습니다.

{% embed url="https://expressjs.com/ko/starter/hello-world.html" %}

```javascript
import express from 'express';

const port = 3000;
const app = express();

// get products collection 구현 코드
app.get('/products', (req, res) => {

    const products = [
        {
            category: 'Fruits', price: '$1', stocked: true, name: 'Apple',
        },
        {
            category: 'Fruits', price: '$1', stocked: true, name: 'Dragonfruit',
        },
        {
            category: 'Fruits', price: '$2', stocked: false, name: 'Passionfruit',
        },
        {
            category: 'Vegetables', price: '$2', stocked: true, name: 'Spinach',
        },
        {
            category: 'Vegetables', price: '$4', stocked: false, name: 'Pumpkin',
        },
        {
            category: 'Vegetables', price: '$1', stocked: true, name: 'Peas',
        },
    ];

    res.send({ products });
});

//3000번 폰트에 연결
app.listen(port, () => {
  console.log(`Example app listening on port ${port}`);
});
```

### URL 구조

URL이란 네트워크 상에 웹 페이지, 이미지 등 자원이 어디에 있는 지 알려주는 웹 주소입니다.

#### URL 구성요소

URL은 프로토콜과 도메인, path, parameter, fragment로 이우어져 있습니다.

* 프로토콜: http 혹은 https 부분으로 서버와 클라이언트간에 request/response를 위한 통신 규약입니다. https는 http 프로토콜의 암호화된 버전을 의미합니다.
* 도메인: 도메인은 컴퓨터의 IP 주소를 사람이 인식하기 쉽도록 이름을 붙여서 만든 웹 주소로 naver, google 등이 이에 해당됩니다.
* path(/): path는 도메인 주소가 가리키고 있는 서버 컴퓨터내 자원의 위치를 의미합니다. 서버 컴퓨터에는 웹 화면을 만들기 위한 html 문서, 이미지, css 파일, js 파일 등의 자원이 존재합니다. path 이러한 자원이 어디있는 지를 의미하는 것으로, 일반적으로 프로젝트의 파일 구조와 동일한 구조를 지니고 있으며 서버의 controller는 이 path를 확인하고 해당되는 웹 화면을 response 합니다.
* parameter(?): 파라미터는 데이터를 다루는 쿼리에 관한 문자열로, ? 뒤에 key=value 형태로 나열되고 & 기호로 구분됩니다. HTTP Method 중 GET 방식을 사용하면 데이터 정보가 url에 파라미터로 나타나며, POST 방식을 사용하면 보안을 위해 나타나지 않습니다.
* fragment(#): 한 페이지 내에서 특정 요소를 지정하고 싶을 경우 사용되는 부분으로 앵커라고도 부릅니다. 특정 요소의 id값을 넣어두면 페이지 랜딩 시 해당 부분에 바로 스크롤이 이동됩니다.

## 강의를 들으면서 든 생각

백엔드에서 사용되는 많은 용어들을 들어보기는 했으나 명확하게 어떤 내용인지는 이해하기 못하고 있다는 것을 깨달았다. 용어를 잘 모르니 기술에 대한 설명 또한 명확하게 이해하기 어려워져서 이 용어들도 같이 공부해야겠다고 생각했다.

### 추가로 공부할 것

node.js란 웹 애플리케이션 프레임워크란 API란 미들웨어란
