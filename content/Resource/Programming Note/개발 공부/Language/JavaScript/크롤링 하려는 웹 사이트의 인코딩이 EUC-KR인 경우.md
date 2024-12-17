---
Create at: 2023-11-23
tags:
  - troubleshooting
---
---
1. 국내 사이트 크롤링시 처음 겪었던 일이다.
2. 증상: 수집한 데이터를 확인하려는데 한글일 경우 `占쏙옙` 이라는게 반복되어 표시됨
3. 관련 환경: Node.js, axios, cheerio.
4. 원인: head에 charset이 UTF-8이 아닌 EUC-KR로 설정되어 있을 경우
5. 해결 방법: 2가지가 있다.
	1. 외부 라이브러리 설치(iconv-lite)
	2. Node.js 버전이 11 이상일 경우 내부 클래스 사용 (TextDecoder)
	```js
	// case 1
const axios = require("axios");
const cheerio = require("cheerio");
const iconv = require("iconv-lite");

const response = await axios.get(url, { responseType: 'arraybuffer', responseEncoding: 'binary' });
const content = iconv.decode(response.data, 'euc-kr');
```

```js
// case 2: 나는 이 방법을 선택했다.
const decoder = new TextDecoder('euc-kr');
const content = decoder.decode(response.data);
```
https://nodejs.org/api/util.html#class-utiltextdecoder