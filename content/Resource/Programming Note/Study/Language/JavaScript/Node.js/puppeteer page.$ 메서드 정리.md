---
Create at: 2024-01-29
tags:
  - Nodejs
  - browser-control
---
---

### `page.$(selector)`:

이 메서드는 주어진 CSS 선택자에 일치하는 페이지 내의 첫 번째 요소에 대한 ElementHandle을 반환합니다.

```js
const element = await page.$('.class-name');
```

---

### `page.$$(selector)`:

주어진 CSS 선택자에 일치하는 페이지 내의 모든 요소에 대한 ElementHandle 배열을 반환합니다.

```js
const elements = await page.$$('.class-name');
```

---
### `page.$x(xpath)`:

주어진 XPath 표현식에 일치하는 페이지 내의 모든 요소에 대한 ElementHandle 배열을 반환합니다.

```js
const elements = await page.$x('//div[contains(text(), "some text")]');
```

---
### `page.$$eval(selector, pageFunction[, ...args])`

주어진 CSS 선택자에 일치하는 모든 요소를 찾고, 그 요소들에 대해 pageFunction을 실행합니다.
이 메서드는 선택된 요소들에 대해 실행할 함수와 해당 함수에 전달할 인수를 받습니다.

```js
const text = await page.$$eval('.class-name', elements => elements.map(e => e.textContent));
```

---
### `page.$eval(selector, pageFunction[, ...args])`:

주어진 CSS 선택자에 일치하는 첫 번째 요소를 찾고, 그 요소에 대해 pageFunction을 실행합니다.
`$$eval`과 비슷하지만, 단일 요소에 대해서만 작동합니다.

```js
const text = await page.$eval('.class-name', e => e.textContent);
```

---
### `page.$$(selector)`와 `elementHandle.$$`의 차이:

`page.$$`는 페이지의 전역 컨텍스트에서 CSS 선택자에 일치하는 모든 요소를 찾습니다.
`elementHandle.$$`는 특정 ElementHandle 내에서 주어진 CSS 선택자에 일치하는 모든 자식 요소를 찾습니다.