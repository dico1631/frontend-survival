# 2주차 1강_jsx(작성중)

## React에서 JSX를 사용하는 목적

jsx는 react에서 element를 제공합니다. react와 jsx가 반드시 같이 사용되어야 하는 것은 아닙니다. jsx 없이 react를 사용할 수 있으며, react 없이 jsx도 사용 가능합니다. 다만 jsx를 쓰고 babel과 같은 도구를 사용하면 자동으로 js로 변환해주어 react를 보다 쉽게 사용할 수 있습니다. 따라서 jsx는 react 사용 시 Syntactic sugar를 위해서 사용됩니다.

### Syntactic sugar

문법적 설탕이라는 문구 그대로, 문법적 내용은 그대로인데 사람이 더 직관적으로 코드를 사용할 수 있도록 "달콤하게" 만든다는 의미.

## JSX없이 react하기

jsx로 입력된 코드는 js로 번역이 되어 실행됩니다. 따라서 js와 jsx는 1:1 호환이 가능합니다. react의 작동원리를 명확하게 파악하기 위해서는 우선 jsx없이 react를 해보는 경험이 필요합니다. jsx를 입력하여 js로 해석된 코드를 보고 싶다면 아래 사이트에서 쉽게 확인할 수 있습니다.
[the online Babel compiler](https://babeljs.io/repl/#?presets=react&code_lz=GYVwdgxgLglg9mABACwKYBt1wBQEpEDeAUIogE6pQhlIA8AJjAG4B8AEhlogO5xnr0AhLQD0jVgG4iAXyJA)

### React.createElement

createElement는 언어 그대로 react element를 만들고, 내용을 갱신하는데 사용됩니다. 이 element들은 트리구조를 가지고 있습니다.

- 매개변수: `React.createElement("button", { type: "button", onClick: () => setCount(count + 1) }, "Increase")`
  - tag 자체 or 객체 생성 함수, 속성 dict, tag 안에 text
  - 첫 번째 매개변수인 element의 경우 html tag는 소문자로 시작, 객체 생성 함수 이름은 대문자로 시작

### React Element

element는 react에서 제일 작은 단위로, object입니다. react는 element 단위로 변경된 사항을 파악해 변경사항이 있는 element만 업데이트 하는 방식으로 화면의 정보를 업데이트(리랜더링)합니다. 이러한 방식은 개발자가 모든 항목의 어떤 부분이 변경이 되어야 하는지 일일히 지정하지 않고, 느슨하게 개발을 해도 된다는 장점이 있습니다. 이때 변경 여부를 판별하기 위해서 VDOM을 생성해서 비교하는 방식을 사용합니다.

#### VDOM(Virtual DOM)이란?

- 화면을 변경할 때 화면 전체를 변경하는 것이 아니라, 가상의 돔을 만들어 현재 돔과 비교를 하고 다른 부분만 갱신합니다. 이를 위해 현재 화면과 동일한 가상의 돔을 만들어 메모리에 저장하는데, 이 가상의 돔을 VDOM이라고 합니다.
- VDOM을 만들어서 비교 후 갱신을 하는 구조이기에, 개발 시 어느 부분이 변경되어야 하고 어떤 부분은 안 바꿔도 되는 지를 사람이 고민하지 않아도 됩니다.(개발이 용이함) 이전에 사용되던 Jquery의 경우 selector를 활용하여 li 중 몇 번째 li가 변경되어야 하는 지 지정을 하는 것과 같이, 변경되어야 하는 항목을 정확히 select 해서 업데이트 해 줘야 했습니다. 하지만 react에서는 VDOM을 만들어 현 상태와 비교하는 과정을 통해 li를 creatElement하는 코드만 입력하면, 이전과 내용을 비교해서 변경사항이 생긴 li 내용만 리랜더링합니다.
- 따라서 VDOM은 속도를 현격히 빠르게 만들어주기에 사용하는 것이 아니라, 괜찮은 정도로 충분히 빠르면서 유지보수를 쉽게 해주기에 사용됩니다.

#### DOM이란?

- The Document Object Model
- DOM은 html tag로 이루어진 문서(Document)를 js 등 코드가 수정할 수 있도록 객체(Object)로 만들어 상호작용 할 수 있도록 하는 방식(Model)입니다. DOM은 트리구조로 이루어져 있으며, 최상위에는 `<html>`이 존재합니다. 이는 브라우저에 로드됩니다.

#### DOM과 Virtual DOM의 차이

DOM은 실제 화면에 뿌려지는 웹사이트 화면이며, VDOM은 state 비교를 하기 위해 DOM을 복제해 만들어진 가상의 DOM입니다.

### React StrictMode

StrictMode는 개발 중 발생할 수 있는 잠재적인 문제를 발견할 수 있도록 제공되는 도구입니다. `<React.StricMode></React.StricMode>` 태그 사이에 strict 모드에 두고 싶은 element를 두면 해당 element에 대해 자손까지 검사 후 문제에 대한 경고메세지를 노출합니다.

## 강의를 들으면서 든 생각

지금 회사에서 jquery를 이용해서 프로젝트를 진행 중인데, 이제 react를 도입하려고 하는 중입니다. 지금은 새로 만들어진 소규모 프로젝트에 우선 react를 적용 중인데, 기존의 jquery 프로젝트에도 react를 도입하기 위해서는 모든 프로젝트의 jquery를 걷어내야 할까요? 아니면 신규로 생기는 페이지에만 react를 도입하는 방식으로 천천히 도입해나갈 수 있을까요?

### 새로 안 용어

- XML과 같다 = 열고 닫는 태그 사용(html과 비슷함)