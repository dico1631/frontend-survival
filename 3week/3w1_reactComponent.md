# 3주차 1강_React Component

## 설명

### REST API 와 GraphQL

REST API와 GraphQL은 개발 시 필요한 데이터를 어떻게 DB에 요청하고 받을 지에 대한 방식입니다.

#### REST API 란 무엇인가

Rest API란 컴퓨터 간의 데이터를 서로 요청하고 전달받을 때 안전하게 데이터를 요청하고 받기 위한 인터페이스로, 구성요소는 아래와 같습니다.

1. 규약에 따라 지정된 HTTP URL을 통해 어떤 자원(resource)를 받고 싶은지 명시합니다.
2. HTTP Method를 통해 데이터를 CRUD 중 어떤 동작을 적용하고 싶은지 명시합니다.

일반적으로 데이터 조작은 Creat, Read, Update, Delete 4가지가 가능하며, 이를 합쳐서 CRUD라고 부릅니다.Rest API에서는 아래와 같이 해당 동작을 명명합니다.

- Create : 데이터 생성(POST)
- Read : 데이터 조회(GET)
- Update : 데이터 수정(PUT, PATCH)
- Delete : 데이터 삭제(DELETE)

#### GraphQL은 왜 등장했는가?

Rest API는 resource 대상과 원하는 동작만 명명하면 단일 요청으로 많은 데이터를 불러올 수 있다는 장점이 있습니다. 하지만 아래 2가지 단점을 가지고 있습니다.

1. Rest API는 특정 데이터만 가져오거나, 특정 컬럼만 가져오는 등 필요한 데이터에 대한 세부 지정이 불가능합니다. 따라서 불필요한 데이터까지 모두 가져오게 되면서 속도 및 자원 활용에 비효율성을 초래할 수 있습니다. (Over-Fetching)
2. Rest API는 한 번에 하나의 url만 입력할 수 있습니다. 이는 요청 당 1개의 resource만 지정할 수 있다는 의미입니다. 이로 인해 여러 DB 혹은 table의 데이터를 모두 활용해야 하는 경우 Resource 종류별로 각각 요청을 해야하기에 이럴 경우 여러 차례 요청이 필요하다는 단점이 있습니다. (Under-Fetching)
3. Rest API는 클라이언트에서 데이터가 필요한 기능이 생길 때마다, 해당 행위에 맞는 API를 새로 생성해야 합니다. 따라서 너무 많은 API가 생성되는 경우 소통 및 관련 문서화에 부담이 존재합니다.
4. Rest API의 Over-Fetching과 Under-Fetching으로 인해 데이터를 조작하고 API 응답간 의존성을 핸들링하기 위해 클라이언트의 코드가 점점 더 복잡하고 길어지는 문제가 있습니다.

GraphQL은 이러한 문제점을 보완하기 위해 클라이언트단에서 바로 query를 작성하고 이에 맞는 데이터를 조작하는 방식으로 구현되었습니다. query에 원하는 데이터를 지정하고, 이를 read 할 수 있으며 추가적인 조작을 할 때는 Mutation를 통해 Create, Update, Delete 할 수 있습니다. 이러한 작동방식은 명확하게 클라이언트에서 필요한 최소한의 정보만 별도 요청이 가능하기에 속도 향상 및 자원의 효율적 사용이 가능합니다. 또한 원하는 정보를 하나의 query에 정의하면 여러 DB나 table에 있는 정보로 한번에 가져올 수 있습니다. 게다가 query 자체가 데이터 스키마 정보를 담고 있기에 이것만으로도 데이터 사용방식에 대한 문서가 되어 별도의 문서화 작업이 필요하지 않습니다.

#### GraphQL의 특징(작업 중)

- Graph 자료 구조
- Query(Read), Mutation(Command: Create, Update, Delete), Subscription(Event)

#### GraphQL의 단점(작업 중)

#### REST API vs GraphQL 코드 비교

```text
- 요청
method : get
end-point : https://choseongho93.com/api/user/1 
```

```json
//결과

{
  "name": "nate",
  "height": "187",
  "hair_color": "blond",
  "skin_color": "fair",
  "eye_color": "blue",
  "gender": "male",
  "study": [3,4,21,23,31],
  "created": "2014-12-09T13:50:51.644000Z",
  "edited": "2014-12-20T21:17:56.891000Z",
}
```

```text
- 요청
end-point : https://choseongho93.com/graphql

query {
  users(userId: 1) {
    name
    height
    gender
  }
}
```

```json
//결과

{
  "data": {
    "person": {
      "name": "nate",
      "height": 187,
      "gender": "male"
    }
  }
}
```

#### JSON

REST API이나 GraphQL을 통해 데이터를 넘겨받을 때, 데이터는 json 형태로 전달됩니다. json은 javascript object notation의 약자로, javascript object로 구조화된 데이터를 사람이 이해할 수 있는 방식으로 표현하여 상호 전달이 원활히 이루어질 수 있도록 만든 데이터 표현 방식입니다. key-value형식으로 되어있어 key를 통해 데이터 내용을 읽을 수 있습니다.

### React

FE에서는 REST API이나 GraphQL를 통해 넘어온 json을 가지고 UI를 구성하여 데이터를 사용자에게 보여줍니다. React는 HTML과 유사한 모양의 DSL을 사용하여 선언형으로 UI를 구성합니다. React는 component 단위로 구성됩니다.

#### DSL(Domain-Specific Language)

DSL란 도메인 특화 언어로, 특정 분야나 문제를 해결하기 위해 만들어진 언어입니다. 웹사이트 구성만을 위해 만들어지고 사용되는 HTML이 대표적인 예시입니다. React 또한 javascript로 개발 시 state를 쉽게 업데이트 하기 위해 만들어지고 사용되는 DSL입니다.

#### 명령형 프로그래밍 vs 선언형 프로그래밍

명령형 프로그래밍과 선언형 프로그래밍은 개발을 통해 문제를 해결할 때 how와 what 중 어떤 항목에 집중해서 문제 해결법(개발코드)를 작성할 지에 방법론입니다. 명령형 프로그래밍은 how에 집중하여, 문제를 해결하기 위한 행위를 상세히 지정합니다. 반면 선언형 프로그래밍은 what에 집중하여 문제 해결을 위해 무엇을 수행하는지를 위주로 작성합니다. 예시로 새로운 데이터를 받아와서 list의 내용을 업데이트 하는 상황에 대해, 명령형 방식의 경우 데이터 요청, 데이터 받기, 데이터 확인, 리스트 select, 리스트에서 데이터가 반영되어야 하는 부분 select, 데이터가 어떻게 업데이트 되어야 하는지 명시, 변경사항 화면에 노출 등의 순서로 행위를 상세히 작성합니다. 반면 선언형 방식을 사용하는 React에서는 어떻게 업데이트를 시킬 지를 작성하는 것이 아니라 "데이터가 변경되면 list를 모두 업데이트 시켜줘"라는 코드를 작성합니다. 그러면 React는 자동으로 어떤 항목이 변경되었는 지 확인하여 수정사항을 반영합니다.

#### React component를 만드는 기준

- SRP(단일 책임 원칙): 객체 지향 프로그래밍에서 사용되는 SOLID 원칙 중 하나로, 하나의 React의 컴포넌트는 한 가지의 책임만 가져야하며 완전히 캡슐화되어야 한다는 원칙입니다. React에서는 이러한 단일 책임을 가진 component를 조립하여 하나의 app을 구성합니다.
- CSS → 이미 알고 있는 기준을 재활용. (다시 공부하기)
- Design’s Layer (다시 공부하기)

#### Atomic Design

Atomic Design은 더 이상 쪼개질 수 없는 최소한의 단위까지 쪼개진 component를 조합하여 시스템을 만드는 방식을 말합니다. Atomic는 '원자'로 더이상 쪼개질 수 없는 최소 단위를 의미하는 것으로 React에서는 component들이 이러한 Atomic한 특성을 띄어야 합니다. Atomic Design으로 시스템을 설계하면 모듈을 만들고, 이를 합쳐서 템플릿을 만들고, 이를 모아서 페이지를 만들고, 이를 모아서 하나의 시스템을 만들게 됩니다. 즉 모든 구성요소는 atomic의 집합입니다. React에서는 React component의 기준을 지켜서 component를 만듦으로써 Atomic Design한 시스템을 설계하기를 지향합니다.

#### React component 와 props

React component를 정의할 때에는 만들어진 component를 import한 뒤 `<ReactComponentName />`을 통해 해당 컴포넌트를 사용하게 됩니다. 이때 함수의 매개변수와 같이 component에 속성들을 전달할 수 있습니다. 속성은 `<ReactComponentName type="type1" state="stable" />`와 같이 HTML에서 태그의 속성을 입력하는 것과 같이 작성됩니다. 하지만 이는 HTML의 속성과 다르게 단순히 매개변수를 전달하는 것으로, 사용을 위해서는 component에서 직접 사용하는 코드를 작성해야만 효력을 발휘합니다. 이 속성들은 자동으로 props라는 변수에 할당되어 전달됩니다.

## 강의를 들으면서 든 생각

팀에서도 모듈화를 중시하기에 html, css, js 모두 코드를 복붙만 하면 바로 사용될 수 있도록 개발을 하려고 노력했었습니다. 그리고 그렇게 개발하는 경우 얼마나 더 효율적으로 개발이 가능한지도 팀에서 개발 협업을 하면서 경험할 수 있었습니다. 그렇지만 실제로 새로운 컴포넌트를 만들고 이를 import 할 수 있는 실제 모듈로 만들어서 사용해본 적은 없었기에 이 점이 새로웠고 업무에도 적용하면 좋을 것 같다는 생각이 들었습니다. 현재 개발 시 모듈이 되도록 설계는 하려고 노력하나 import가 되는 실제 모듈로 개발하진 않았기에 결국 상황에 따라 해당 js, css 파일이 import되기 어려운 환경에서는 같은 코드가 여러 파일에 반복되는 상황이 발생할 수 밖에 없었습니다. 또한 html과 type까지 분리해서 atomic하게 사용할 수 있다는 생각까지는 해보지 못했었습니다. 그래서 html의 경우에는 반드시 동일한 코드가 각 파일에 존재할 수 밖에 없다고 생각했습니다. 이번 실습을 통해 react를 활용하면 html까지도 모듈에 포함되는 진짜 모듈이 될 수 있다는 점을 알게 되어 의미가 있었습니다.

### 추가로 공부할 것

객체 지향-solid 원칙
thinking in react docs 읽어보기
js reduce 함수
typescript 사용자 정의 타입, interface
typescript export default
