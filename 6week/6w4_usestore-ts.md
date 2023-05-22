# 6주차 4강 usestore-ts

## usesotre-ts

`@Action`, `@Store()` 등 decorator를 이용하여 자동으로 관련 코드를 만들어주도록 함
비동기 함수에 `@Action`을 넣으면 동작이 끝난 뒤 publich 되리라는 보장을 못하기에 비동기 부분은 따로 함수로 빼서 동작해야 함

- Store 작성.

```tsx
import { singleton } from 'tsyringe';

import { Store, Action } from 'usestore-ts';

@singleton()
@Store()
export default class CounterStore {
    count = 0;

    @Action()
    increase(step = 1) {
        this.count += step;
    }

    @Action()
    decrease() {
        this.count -= 1;
    }
}
```

- 커스텀 Hook 작성.

```tsx
import { container } from 'tsyringe';

import { useStore } from 'usestore-ts';

import CounterStore from '../stores/CounterStore';

export default function useCounterStore() {
    const store = container.resolve(CounterStore);
    return useStore(store);
}
```

- 사용

```jsx
const [state, srore] = useCounterStore();
```

## useSyncExternalStore

- [useSyncExternalStore](https://beta.reactjs.org/reference/react/useSyncExternalStore)
- [FECONF 2022 - 상태관리 이 전쟁을 끝내러 왔다](https://youtu.be/KEDUqA9JeIo)

## 강의를 들으면서 든 생각

수업 때 쓰는 예시 코드를 제공해주면 좋겠다는 생각이 들었다. 쭉 이런 생각이 들고 있었지만, 이번 6주차 3,4강을 들으면서 더 그런 생각이 강하게 들었다.