# 5주차 3강_MSW

## 설명

### Service worker의 필요성

FE 개발 중 겪는 하나의 불편한 점은 백엔드 API가 완성이 되어야지만 개발을 진행할 수 있는 상황이 많다는 것입니다. 프론트엔드에서는 백엔드 API가 완성이 되어야 이를 통해 데이터를 가져오고 화면을 구성할 수 있기 때문에, 이럴 경우 백엔드 개발이 끝나기를 기다릴 수 밖에 없습니다. 또한 서버와의 통신은 반드시 인터넷이 연결되어야 하기에 오프라인 환경에서는 테스트가 불가능합니다. 하지만 프로젝트 기한은 정해져 있기에 더 효율적인 개발을 하기 위해서, 프론트엔드에서 데이터를 요청하면 실제 서버에 요청했을 때와 비슷한 가짜 응답을 전달해주는 API를 만들었습니다. 이러한 개발용 임시 API들을 Service worker라고 하며, MSW가 제일 많이 사용되고 있습니다.

### Service worker란

Service worker는 이전에 말한 것처럼 오프라인 상황이나 백엔드 개발이 미완료 되는 등의 이유로 서버와의 통신을 할 수 없지만 테스트 혹은 구현을 위해 서버와의 통신을 임의로라도 구성할 필요가 있을 때 사용하는 API입니다. Service worker는 서버와 클라이언트 간의 리소스 요청이 있을 경우 네트워크 단계에서 해당 요청을 가로챈 뒤, 요청 내용을 확인하여 임의로 기술된 적절한 모의 request를 서버 대신 내어줍니다. Service worker는 보안 상의 이유로 HTTPS에서만 동작합니다. 네트워크 요청을 수정할 수 있다는 점에서 중간자 공격에 굉장히 취약하기 때문입니다. 또한 Firefox에서는 사생활 보호 모드에서 Service Worker API에 접근할 수 없습니다.<br/>
이러한 mocking 작업은 jest에서도 `jest.mock()`함수를 통해 해당 기능을 제공합니다.

```jsx
import { render, screen } from '@testing-library/react';

import App from './App';

// API 요청을 하는 각 페이지에 API 개수만큼 아래 코드가 작성되어야 한다.
jest.mock('./hooks/useFetchProducts', () => () => [
 {
  category: 'Fruits', price: '$1', stocked: true, name: 'Apple',
 },
]);

test('App', () => {
 render(<App />);

 screen.getByText('Apple');
});
```

하지만 `jest.mock()` 함수는 서버 요청이 있는 모든 파일에서, 요청의 가지 수 만큼 해당 코드를 작성해야 합니다. 따라서 API 종류가 많아지고 요청을 하는 클라이언트 페이지가 많아지면 점점 더 코드가 복잡해지는 단점이 있습니다. 따라서 이러한 mocking 관련 코드를 모두 모아서 관리할 수 있으며 상황에 맞게 적절한 응답을 내보낼 수 있는 모듈형 구조를 만들기 위해 `jest.mock()`대신 MSW 라이브러리를 사용합니다.

### MSW(Mock Service Worker)

MSW란 Service Worker를 이용해 서버를 향한 실제 네트워크 요청을 가로채서(intercept) 모의 응답 (Mocked response)를 보내주는 API Mocking 라이브러리입니다. MSW를 사용하면 직접 Mock 서버를 구현하지 않아도, 네트워크 수준에서 API를 Mocking 할 수 있습니다. Mocking 테스트를 위한 노드(node.js)환경, 개발 및 디버깅을 위한 브라우저 환경에서 모두 사용할 수 있다는 장점이 있다. 또한, 소스 코드 수정 없이 모킹이 필요한 환경에서만 MSW 인스턴스를 실행해 API Mocking을 적용할 수 있습니다.

- MSW 패키지 설치: `npm i -D msw`

#### MSW 구조

MSW를 통해 테스트 환경을 구축하기 위해서는 아래 4개의 파일이 필요합니다.

1. 테스트 사전 준비 및 테스트 완료 후의 액션이 정의된 `src/setupTests.ts`
2. request에 따라 어떤 모의 응답을 내어 줄 지가 정의된 `src/mocks/handlers.ts`
3. setupTest과 handlers를 연결하는 `src/mocks/server.ts`
4. 실제 구현된 app에 대한 테스트 코드 `App.test.ts`

- `src/setupTests.ts` 파일

우선 mocking을 위해 사전 준비 단계가 필요합니다. 우선 모든 테스트를 시작하기 전에는 server를 켜야하고, 테스트가 종료되면 server를 닫아야 합니다. 그리고 각 테스트 시작 전에는 지금 진행될 테스트에 맞도록 handlers를 세팅해야 합니다. handlers는 API url과 http method를 활용하여 어떤 답변을 줄 지가 작성되어 있는 코드입니다.

  ```jsx
  import server from './mocks/server';

  beforeAll(() => server.listen({ onUnhandledRequest: 'error' }));

  afterAll(() => server.close());

  afterEach(() => server.resetHandlers());
  ```

- `src/mocks/handlers.ts` 파일

API는 요청하는 자원의 위치 정보가 담긴 url과 CRUD 중 어떤 동작을 요청하는 지가 담긴 Http Method로 구성되어 있습니다. mock의 handelers도 동일하게 url와 Http Method에 따라 어떤 return을 내어줄 지 분기처리가 되어있습니다. 아래 코드는 `http://localhost:3000/products` URL에 get 요청을 했을 경우 response로 products 객체를 return합니다.

  ```jsx
  import { rest } from 'msw';

  const BASE_URL = 'http://localhost:3000';

  const handlers = [
    rest.get(`${BASE_URL}/products`, (req, res, ctx) => {
      const products = [
        {
          category: 'Fruits', price: '$1', stocked: true, name: 'Apple',
        },
      ];

      return res(
        ctx.status(200),
        ctx.json({ products }),
        );
      }),
    ];

  export default handlers;
  ```

- `src/mocks/server.ts` 파일

`server.ts` 파일에서는 msw의 setupServer 함수에 `src/mocks/handlers.ts`를 매개변수로 넣음으로써 `src/setupTests.ts`에서 사용하는 server를 만들어서 export합니다.

  ```jsx
  import { setupServer } from 'msw/node';

  import handlers from './handlers';

  const server = setupServer(...handlers);

  export default server;
  ```

- mocking 활용을 위한 app 테스트 파일

이전까지 3개의 파일은 테스트를 위해 mocking 준비를 한 내용이었습니다. `App.test.ts`은 실제 구현한 app에 대한 테스트 코드가 작성된 파일로 이 곳에서 서버와의 통신이 필요한 테스트를 할 때 위에 정의된 mocking 서버를 활용하게 됩니다.

<br/><br/>

app에는 아래와 같이 fetch API를 사용하는 코드가 작성되어 있습니다. `http://localhost:3000/products` url에 get을 요청하여 product 배열을 전달받고 있습니다. 여기서 사용된 `http://localhost:3000/products` URL과 get 함수가 바로 `src/mocks/server.ts` 파일에서 정의한 API입니다. 이제 아래 코드를 `App.test.ts` 파일에서 테스트하게 되면 해당 코드가 실행될 때 실제 서버에 요청을 하는 것이 아니라 mocking으로 만들어진 서버 요청을 해서 모의 응답이 돌아올 것입니다.

- `useFetchProduct.ts` 파일

  ```jsx
  import {useEffect, useState} from 'react';

  import type Product from '../type/Product';

  export default function useFetchProducts() {
    const [products, setProducts] = useState<Product[]>([]);

    useEffect(() => {
      const fetchProducts = async () => {
        const url = 'http://localhost:3000/products';
        const response = await fetch(url);
        const data = await response.json();
        setProducts(data.products);
      };

      fetchProducts();
    }, []);

    return products;
  }
  ```

- `App.test.ts` 파일

  ```jsx
  import { render, screen, waitFor } from '@testing-library/react';

  import App from './App';

  test('App', async () => {
    render(<App />);

    await waitFor(() => {
      screen.getByText('Apple');
    });
  });
  ```

**polyfill(폴리필)**: `useFetchProduct.ts` 파일에서 사용된 fetch 함수는 실행 환경인 window 브라우저에는 있으나 개발 환경인 node에는 없는 함수입니다.따라서 fetch 함수로 오류가 날 경우 polyfill 라이브러리를 설치하면 정상적으로 작동합니다.<br/>(최신 버전의 node에는 fetch 함수가 포함되어 있어, 이와 같은 추가 작업이 필요하지 않습니다.)

- [GitHub에서 만든 fetch polyfill](https://github.com/github/fetch)

## 강의를 들으면서 든 생각

지금 일하는 회사에서는 프론트엔드 팀이 생긴 지 얼마되지 않았고, 모두 연차가 낮다는 이유로 보통 프론트엔드에서 하드코딩으로 디자인에 맞춰서 화면을 구현 후 백엔드에 전달하기만 한다. 그리고 백엔드에서 API를 만들어 데이터를 넣는 작업까지 모두 진행하고 있다. 그런데 프론트엔드에서 백엔드 API 개발 기간동안 대기할 수 없어 임의의 서버까지 구현하면서 개발을 한다는 사실을 알고, 조금 충격을 먹었다. 이번 수업을 들으면서 다시 한번 나는 우물 안 개구리고, 이대로 있으면 온실 속 화초같이 길러지겠다는 생각이 들었다. 이번 강의는 공부에 대한 목적 의식을 한 번 더 불태우게 만드는 수업이었다.

### 추가로 공부할 것

프록시 서버
