# 5주차 4강\_Playwright

## 설명

### E2E(End to End) Test

유닛 테스트와 통합 테스트 후 최종 인수 테스트 단계를 말하는 것으로, app의 흐름을 처음부터 끝까지 테스트 하는 것을 의미합니다. 유닛 테스트와 통합 테스트가 모듈의 무결성을 검증하는 단계라면 이는 애플리케이션의 무결성을 검증하기 위한 단계입니다. 이를 위해서 실제 서비스 환경에서의 유저 플로우를 테스트에 적용합니다.

#### E2E Test 장단점

1. QA팀의 업무를 줄여줄 수 있는 좋은 수단이 될 수 있습니다. QA에서는 기획안을 기준으로 시나리오를 짜고 이를 테스트하면서 검증하는 작업을 합니다. 따라서 E2E Test를 업무의 좋은 보조수단으로 사용할 수 있습니다. 그러나 QA팀과의 업무 협의가 이루어지지 않은 상황에서 사용할 경우 애매한 위치가 되어버릴 수 있습니다. 개발자가 A-D까지의 시나리오를 짜서 테스트를 했다고 하더라도 더 전문성이 있는 QA팀에서는 A-Z 혹은 그 이상의 시나리오를 테스트하고 있을 수 있습니다. 이렇게 되면 E-Z에는 오류가 발생하게 되고, QA팀은 이미 시나리오 기반으로 테스트가 된 A-D도 재테스트를 하고 있는 상황이 발생하면서 결국 더 큰 비효율이 초래될 수 있습니다.
2. 테스트 코드는 프로젝트 진행 중 뿐만 아니라 유지 보수 중에도 예측하지 못하는 side-effect나 오류를 잡을 수 있게 합니다. 그러나 이를 최신 버전에 맞게 관리하는 것은 상당한 시간과 비용이 듭니다. 게다가 관리가 되지 않을 경우 오히려 개발에 방해가 될 수 있습니다.
3. E2E Test는 유닛 테스트나 통합 테스트와 다르게 API 호출과 더불어 화면 랜더링, 실제 동작 등의 GUI 활동까지 테스트 합니다. 따라서 훨씬 테스트 진행에 더 많은 시간이 소요됩니다.

### Playwright

웹 브라우저 기반 E2E 테스트를 자동화하는 도구입니다. Headless Chrome을 기반으로 한 Puppeteer를 계승하면서, 더 많은 웹 브라우저를 지원합니다.

#### Puppeteer

Headless Chrome나 Chrominum을 제어할 수 있는 구글의 노드 라이브러리입니다. 화면 스크린샷 및 PDF 저장, UI 테스트 자동화 등을 할 수 있습니다.

#### Headless Chrome

Headless Chrome은 브라우저 화면이 뜨지 않고 인터넷 상의 행동을 할 수 있도록 하는 모드입니다. 이 모드를 실행하시면 인터넷 창을 키지 않고 GUI 테스트까지 진행하기에 속도가 조금 더 빠르고, 커서의 포커스가 옮겨가는 등의 상황이 발생하지 않아 다른 작업을 하는데 방해가 되지 않습니다. 다만 화면으로 테스트되는 모습을 확인할 수는 없습니다. 아래 `playwright.config.ts` 파일에 나와있는 것처럼 시작 시 CI='문자열'을 입력하시면 Headless 모드로 테스트가 실행됩니다.

#### 사용법

* 테스트 도구 설정 파일: `playwright.config.ts` 파일

```jsx
import { PlaywrightTestConfig } from '@playwright/test';

const config: PlaywrightTestConfig = {
    testDir: './tests',
    retries: 0,
    use: {
    baseURL: 'http://localhost:8080',
        headless: !!process.env.CI,
        screenshot: 'only-on-failure',
    },
};

export default config;
```

* 테스트 시나리오 파일: `tests/home.spec.ts`

```jsx
import { test, expect } from '@playwright/test';

test('Filter products', async ({ page }) => {
    await page.goto('/');

    await expect(page.getByText('Apple')).toBeVisible();

    const searchInput = page.getByLabel('Search');

    await searchInput.fill('a');

    await expect(page.getByText('Apple')).toBeVisible();

    await searchInput.fill('aa');

    await expect(page.getByText('Apple')).toBeHidden();
});

test('Click the “Increase” button', async ({ page }) => {
    await page.goto('/');

    const count = 13;

    await Promise.all((
        [...Array(count)].map(async () => {
            await page.getByText('Increase').click();
        })
    ));

    await expect(page.getByText(`${count}`)).toBeVisible();
});
```

* 테스트 실행: `npx playwright test`

### CodeceptJS

syntactic sugar를 위한 도구로 더 사람 친화적으로 이해하기 쉬운 코드로 테스트 시나리오를 짤 수 있도록 해준다. 어떤 테스팅 도구 기반으로 돌아갈 지도  선택 할 수 있다. 단순한 기능 테스트 시 유용하며, 복잡할 떈 부족한 면이 있다.

* 사용 코드 예시

```jsx
Feature('Google');

Scenario('Search “CodeceptJS”', ({ I }) => {
  I.amOnPage('https://www.google.com/ncr');
  I.fillField('[name="q"]', 'CodeceptJS');
  I.click('Google Search');
  I.see('codecept.io');
});
```

## 강의를 들으면서 든 생각

과제에서 테스트를 위해 돌렸던 코드가 어떤 내용인지 이해할 수 있었다. 또한 작년 프로젝트 때 input폼을 수동으로 테스트하기 위해 많은 시간과 노력을 들였는데, 테스트 코드가 있었더라면 그렇게 반복 작업을 하지 않아도 되었을 것 같다는 생각이 든다. 또한 비밀번호 노출 기능을 만들면서 뒤에 있을 side-effect 오류를 생각하지 못해 많은 오류가 났었는데, 해당 폼들을 미리 E2E 테스트 코드로 만들었더라면 사전에 확인할 수 있었을 거라는 아쉬움이 남는다.

### 추가로 공부할 것

* CI 환경
* Promise
* Promise.all(): 내부 동작이 모두 성공적으로 종료된 뒤에 다음 것이 실행되도록 함

```jsx
await Promise.all((
    [...Array(count)].map(async () => {
        await page.getByText('Increase').click();
    })
));
```
