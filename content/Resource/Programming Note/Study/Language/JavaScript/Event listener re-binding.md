---
tags:
  - Javascript
---


- 증상: html 페이지를 처음 로드 후 존재하지 않았던 이벤트 리스너 혹은 추가 html 요소 기능이 제대로 초기화 되거나 바인딩 되지 않았을경우 html 요소의 기능이 먹통이 되는 증상을 발견.
- 해결 방법: 새 요소가 추가된 이후의 DOM에 기능을 바인딩 하면 해결할 수 있음

```js
// from
$("button[type=submit]").on("click", (event) => {		(contents)}
// to
$(document).on("click", ".btn-primary", () => {		(contents)}
```

  

```js
$(document).on("DOMNodeInserted", () => {  
// 바인딩하고자 하는 요소의 선택자를 사용하여 이벤트를 바인딩.  
	$(".btn-primary").on("click", () => {    
		// 이벤트 핸들러  
		});
});
```