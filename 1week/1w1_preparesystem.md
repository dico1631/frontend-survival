# 1주차 1강\_개발환경 세팅

우선 온라인 공식몰을 만들기 위해 필요한 개발환경에 대해 알아봅시다. 개발환경은 기술과 환경이 바뀌면서 언제든 변경될 수 있습니다. 따라서 전체 리스트를 외우는 것보다는 각각의 정의와 역할을 숙지하여 언제든 상황에 맞는 개발 도구로 변경할 수 있도록 공부하는 것이 중요합니다.&#x20;

## Node.js

> 참고영상: 유튜브 우아한테크 [\[10분 테코톡\] 유세지의 Node.js](https://www.youtube.com/watch?v=A04zlpL1Uw4\&pp=ygUHbm9kZSBqcw%3D%3D)

node.js는 V8으로 빌드된 이벤트 기반 자바스크립트 런타임입니다. 초장기 js는 html을 조금 더 다양하게 만들기 위해 만들어져 오로지 브라우저에서만 동작이 되었습니다. 크롬, 사파리, 파이어폭스 등 브라우저는  js를 해석해서 실행해주는 프로그램을 내장하였고 이를 통해 js가 실행이 되었습니다. 이때 크롬에서 사용한 js 해석 프로그램이 V8이며, 이러한 실행 프로그램을 런타임이라고 합니다. V8은 타 브라우저의 js 런타임보다 속도와 기능이 훨씬 우수했습니다. 그래서 크롬은 이를 브라우저에만 한정하지 않고 별도의 프로그램으로 분리하여 출시했고 이 프로그램이 node.js입니다. 따라서 mode.js는 브라우저가 아닌 곳에서도 js를 사용할 수 있도록 해주는 프로그램, 런타임입니다. 대표적인 예로 node.js를 깔면 cmd창에서도 console.log와 같은 js를 사용할 수 있습니다.

### Node.js의 특징

#### Non-blocking I/O

#### Event-driven

### NPM(Node Package Manager)

Node.js의 패키지를 관리할 수 있는 도구

* package.json: 패키지 목록과 이름, 버전, 설명, 작성자, 라이센스 등 기본 정보가 담긴 json 파일
* package-lock.json: npm을 사용해서 node\_modules 트리나 package.json 파일을 수정하게 되면 자동으로 생성되는 파일
* npx: node\_modules/.bin 내에 있는 것을 실행하는 경우 사용하는 명령어, `npm i -D`로 설치를 하면 npx로 실행 로컬에 설치되지 않은 모듈도 불러와서 실행시켜 줌. 단, 로컬 모듈을 실행할 때 더 빠름

#### ES Modules vs CommonJS

모듈을 불러오는 방식

* CommonJS: require/exports
  * require()를 동기에서 실행, 읽은 즉시 실행
* ES Modules: import/export
  * 모듈 로더를 비동기 환경에서 실행
  * 스크립트를 바로 실행하지 않고 import/export를 찾아서 스크립트 파싱 후 import할 것이 없을 때까지 import 구문을 찾아서 실행 준비를 한 뒤 실행됨
