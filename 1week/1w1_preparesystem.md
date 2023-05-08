# 1주차 1강_개발환경 세팅

## 키워드

- Node.js
- NPM(Node Package Manager)
  - package.json / package-lock.json
  - node_modules
  - npx
- ES Modules vs CommonJS

## 설명

- 개발 환경은 수시로 변경되기에 이에 대한 대응 능력을 키워야 한다. 이를 위해서는 각각의 역할과 왜 설정을 했는지 목적을 생각하면서 환경설정을 진행해야 한다.

### Node.js

- js를 브라우저 말고 로컬 pc에서도 실행시켜줄 수 있도록 한 js의 런타임(환경, 프로그램)

#### NPM(Node Package Manager)

Node.js의 패키지를 관리할 수 있는 도구

- package.json: 패키지 목록과 이름, 버전, 설명, 작성자, 라이센스 등 기본 정보가 담긴 json 파일
- package-lock.json: npm을 사용해서 node_modules 트리나 package.json 파일을 수정하게 되면 자동으로 생성되는 파일
- npx: node_modules/.bin 내에 있는 것을 실행하는 경우 사용하는 명령어, `npm i -D`로 설치를 하면 npx로 실행 로컬에 설치되지 않은 모듈도 불러와서 실행시켜 줌. 단, 로컬 모듈을 실행할 때 더 빠름

#### ES Modules vs CommonJS

모듈을 불러오는 방식

- CommonJS: require/exports
  - require()를 동기에서 실행, 읽은 즉시 실행
- ES Modules: import/export
  - 모듈 로더를 비동기 환경에서 실행
  - 스크립트를 바로 실행하지 않고 import/export를 찾아서 스크립트 파싱 후 import할 것이 없을 때까지 import 구문을 찾아서 실행 준비를 한 뒤 실행됨
