# 2주차 1강_jsx(작성중)

## 키워드

### React에서 JSX를 사용하는 목적

jsx는 react에서 element를 제공합니다. react와 jsx가 반드시 같이 사용되어야 하는 것은 아닙니다. jsx 없이 react를 사용할 수 있으며, react 없이 jsx도 사용 가능합니다. 다만 jsx를 쓰고 babel과 같은 도구를 사용하면 자동으로 js로 변환해주어 react를 보다 쉽게 사용할 수 있습니다. 따라서 jsx는 react 사용 시 Syntactic sugar를 위해서 사용됩니다.

#### Syntactic sugar

문법적 설탕이라는 문구 그대로, 문법적 내용은 그대로인데 사람이 더 직관적으로 코드를 사용할 수 있도록 "달콤하게" 만든다는 의미.

### JSX없이 react하기

jsx로 입력된 코드는 js로 번역이 되어 실행됩니다. 따라서 js와 jsx는 1:1 호환이 가능합니다. react의 작동원리를 명확하게 파악하기 위해서는 우선 jsx없이 react를 해보는 경험이 필요합니다. jsx를 입력하여 js로 해석된 코드를 보고 싶다면 아래 사이트에서 쉽게 확인할 수 있습니다.
[the online Babel compiler](https://babeljs.io/repl/#?presets=react&code_lz=GYVwdgxgLglg9mABACwKYBt1wBQEpEDeAUIogE6pQhlIA8AJjAG4B8AEhlogO5xnr0AhLQD0jVgG4iAXyJA)

#### React.createElement

createElement는 언어 그대로 react element를 만들고, 내용을 갱신하는데 사용됩니다. 이 element들은 트리구조를 가지고 있습니다.

- 매개변수: `React.createElement("button", { type: "button", onClick: () => setCount(count + 1) }, "Increase")`
  - tag 자체 or 객체 생성 함수, 속성 dict, tag 안에 text
  - 첫 번째 매개변수인 element의 경우 html tag는 소문자로 시작, 객체 생성 함수 이름은 대문자로 시작

### React Element

### React StrictMode

### VDOM(Virtual DOM)이란?

- 화면을 변경할 때 화면 전체를 변경하는 것이 아니라, 가상의 돔을 만들어 현재 돔과 비교를 하고 다른 부분만 갱신한다. 이를 위해 현재 화면과 동일한 가상의 돔을 만들어 메모리에 저장하는데, 이것이 VDOM이다.
- 4VDOM을 만들어서 비교 후 갱신을 하는 구조이기에, 개발 시 어느 부분이 변경되어야 하고 어떤 부분은 안 바꿔도 되는 지를 사람이 고민하지 않아도 됨. 따라서 개발이 용이해짐.
  - Jquery의 경우 변경되어야 하는 항목을 정확히 select 해서 업데이트 해 줘야 함.
  - 빨라서 쓰는 것 X, 괜찮은 정도로 충분히 빠르면서 유지보수를 쉽게 해줘서 쓰는 것

#### DOM이란?

#### DOM과 Virtual DOM의 차이

### Reconciliation(재조정) 과정은 무엇인가?


## 설명

## 강의를 들으면서 든 생각

### 개념 공부하기

문법적 설탕
XML과 같다 = 열고 닫는 태그 사용(html과 비슷함)
바벨: jsx를 js 코드로 변환해줌
패턴
선언적 API
1 2 3 4 5 6 > 2 3 4 5 6 7 에서 1이 없어지고, 7이 생겼다는 것을 알도록 key를 사용함
