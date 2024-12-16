---
Create at: 2023-12-12
tags:
  - Javascript
---
---
1. 현재 상황
	1. `IntersectionObserver` 메서드로 해당 클래스가 붙은 영역을 관찰하고 있다가
		2. 뷰포트에 들어오면 해당 클래스를 붙이고
		3. 뷰포트 벗어나면 해당 클래스를 제거 하고.
		4. 그래서 뷰포트 들어올때마다 애니메이션이 반복 재생된다.
	2. 해결 방안
		1. 기존 css 모두 미사용처리, 클래스 이름만 사용.
		2. 처음 opacity css를 0으로 처리
		3. 뷰포트 들어오면 opacity를 1로 처리하는 방법으로 간단하게 구현.
		4. 단 fade-in 같은건 제외 시켜버림

```js
/* 
  뷰포트를 감지하고 애니메이션을 제어하는 코드
*/
let lastScroll = 0;

function observeClassOnIntersection(className) {
  const targets = $(`.${className}`);
  const observer = new IntersectionObserver(onIntersection, {
    root      : null,
    rootMargin: '0px',
    threshold : 0.8
  });

  targets.each(function() {
    observer.observe(this);
  });

  function onIntersection(entries) {
    entries.forEach(entry => {
      const $target = $(entry.target);
      if (entry.isIntersecting) {
          $target.animate({'opacity': '1'}, 700)
          $target.removeClass(className);
      }
    });
  }
}

// 원래 해당 이름의 css가 있었지만 주석처리한 상태
const classesToObserve = ['slide-in-right', 'fade-in', 'slide-in-left', 'roll-in-left', 'fade-in-top','fade-in-bottom','fade-in-left','fade-in-right'];

classesToObserve.forEach((className) => {
  if ($(`.${className}`).length > 0) {
    observeClassOnIntersection(className);
  }
});
```