# 8주차 1강\_Design System

웹 사이트에 접속하는 디바이스가 다양해짐에 따라 반응형 웹 사이트을 구축해야 합니다. 또한 로직 뿐만 아니라 컴포넌트와 디자인 요소 또한 중복이 없도록 만들어야 합니다.

## 반응형 웹 디자인(Responsive web design)

pc 외에도 태블릿, 모바일 등 웹에 접근하는 디바이스가 다양해지면서 각 기기의 viewpoint width에 맞춰 최적의 화면을 보여줄 필요가 있어졌습니다. 그래서 px 단위를 활용하여 정교하게 하나의 디바이스에 맞춰진 웹 사이트를 구축하는 대신 em, rem, % 등 상대적인 단위를 활용하고, @media 코드로 화면 구조를 변경하면서 웹 사이트를 구성하게 되었습니다. 이처럼 고객이 어떤 디바이스를 통해 어느 정도 크기로 화면을 보고있는지에 반응하여 최적의 display를 위해 화면이 변경되는 웹을 반응형 웹이라고 합니다.

## 디자인 시스템(Design System)

디자인 시스템이란 큰 규모의 시스템을 만들 때 중복을 제거하기 위해서 컴포넌트 혹은 패턴을 기준에 맞춰서 사용하는 방식을 말합니다. 일반적으로 웹 사이트를 디자인할 때는 사이트의 통일감을 위해 같은 기능을 가진 항목들은 동일한 디자인을 가지게 됩니다. 이를 개발할 때는 컴포넌트 뿐만 아니라 css 또한 패턴화하여 중복이 없도록 개발이 되어야 합니다. 이를 위해 필요한 것이 디자인 시스템입니다.

- [Laura Kalbag의 “Design Systems” 소개](https://24ways.org/2012/design-systems/)
- [Laura Kalbag의 “Design Systems” 슬라이드](https://speakerdeck.com/laurakalbag/design-systems-1)
- [Nielsen Norman Group의 “Design Systems 101”](https://www.nngroup.com/articles/design-systems-101/)

- 디자인 시스템 사례
  - [Atlassian Design System](https://atlassian.design/)
  - [Material Design (Google)](https://material.io/)
  - [Base Web (Uber)](https://baseweb.design/)
  - [Polaris (Shopify)](https://polaris.shopify.com/)
  - [Lightning Design System (Salesforce)](https://www.lightningdesignsystem.com/)
  - [Mailchimp Pattern Library](https://ux.mailchimp.com/patterns)
  - [Ant Design](https://ant.design/)

### Atomic Design

그러면 디자인 시스템을 활용하여 중복이 없도록 개발을 할 때 어느정도 단위까지 항목을 쪼개서 컴포넌트를 만들어야 할까요? 그 기준은 [Atomic Design](https://bradfrost.com/blog/post/atomic-web-design/)을 통해 알 수 있습니다. Atomic Design은 Atomic해질 수 있는 더 이상 쪼갤 수 없는 최소 단위까지 항목을 나누어 컴포넌트를 구축해야 한다고 하고 있습니다. 그리고 이러한 atoms를 모아서 모듈이 만들어지고, 이들이 모여서 템플릿, 페이지 최종적으로 하나의 시스템을 구축하는 블럭과 같은 구조가 되는 것입니다.

- [Atomic Design 소개 글](https://bradfrost.com/blog/post/atomic-web-design/)
- [Atomic Design 전자책](https://atomicdesign.bradfrost.com/)

## 강의를 들으면서 든 생각

2강에 작성

### 추가로 공부할 것
