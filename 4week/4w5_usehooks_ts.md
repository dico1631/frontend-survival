# 4주차 5강_usehooks-ts

## 설명

### usehooks-ts

usehooks-ts는 유용한 React hook이 모여있는 라이브러리입니다. usehooks-ts의 hook을 활용하면 더 다양한 기능을 할 수 있을 뿐만 아니라, 기존 react의 hook을 사용했을 때보다 더 명확하게 코드의 의도를 표현할 수 있습니다. ts와 js에서 모두 사용 가능합니다. document에 구현 로직 코드까지 바로 들어있기에 이를 통해 내부 로직을 바로 파악할 수 있고, 나만의 hook을 만들 때 참고하기도 좋습니다. usehooks-ts를 import하면 내부의 hook들을 사용할 수 있습니다.

- [usehooks-ts document](https://usehooks-ts.com/react-hook/use-boolean)

#### useBoolean

useState를 통해서도 state 값을 할당할 수 있으나, 이는 다양한 type이 가능합니다. 만일 해당 state가 반드시 Boolean 값만을 가지는 것이 확실하다면 useBoolean hook을 통해 타입이 더 명확한 코드를 작성할 수 있습니다.

```jsx
// toggle을 하면 isTrue가 변경됨
// value와 toggle에 명시적 이름을 주고 싶다면 : 뒤에 이름을 작성하면 됨
const { value: isTrue, toggle } = useBoolean(true);
```

#### useEffectOnce

useEffect에 의존성 배열을 넣어 1회만 실행되게 하는 경우 이 hook을 사용하면 더 명확하게 의도를 표현할 수 있다.

#### useFetch

더 짧은 코드로 쉽게 fetch를 쓸 수 있습니다. 정말 간단한 코드를 사용할 때는 유용합니다. 다만 기능이 별로 없어서 캐시 이슈까지 다룰 수 있는 [SWR](https://swr.vercel.app/ko)나 [react-query](https://tanstack.com/query) 라이브러리이 더 많이 사용됩니다.

#### useInterval

setInterval의 비동기 실행으로 인해 생기는 문제를 해결해주는 코드까지 담긴 라이브러리입니다.

- [useEffect 관점](https://overreacted.io/ko/a-complete-guide-to-useeffect/)
  - [React에서의 타이머 part 1 : setInterval 말고 이것! - 코드종님 영상](https://youtu.be/2tUdyY5uBSw)
- [Ref 활용](https://overreacted.io/making-setinterval-declarative-with-react-hooks/)

#### useEventListener(작성 중)

#### useLocalStorage(작성 중)

#### useDarkMode(작성 중)

## 강의를 들으면서 든 생각

hook이 정말 많아서 활용을 잘하기 위해서는 많이 써봐야겠다는 생각이 들었다.

### 추가로 공부할 것
