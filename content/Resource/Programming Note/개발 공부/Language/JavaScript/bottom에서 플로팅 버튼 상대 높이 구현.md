
---

```html
<div class="offset">
	<%- include('./contact-us') %>
	<%- include('./map-and-scrollUp') %>
	<%- include('./footer') %>
</div>
```
- 전체 높이를 쉽게 구하기 위해 `.offset` 클래스로 감싸기

```javascript
$(window).on('scroll', function () {
	const elementOffsetTop = $(".offset").offset().top;
	const elementHeight = $(".offset").height();
	const windowHeight = $(window).height();
	const scrollPosition = $(window).scrollTop();

	const elementBottom = elementOffsetTop + elementHeight;

	// 영역이 뷰포트에 얼마나 보이는지 계산
	const visibleTop = Math.max(0, elementOffsetTop - scrollPosition);
	const visibleBottom = Math.min(windowHeight, elementBottom - scrollPosition);

	const visibleHeight = Math.max(0, visibleBottom - visibleTop);
	const $floating = $('.floating');
	// 결과를 활용
	if (visibleHeight > 0) {
		$floating.addClass('on');
		$floating.css({ "bottom": visibleHeight + 30 + "px" });
	} else {
		$floating.removeClass('on');
		$floating.css({ "bottom": 30 + "px" });
	}
});
```

- 현재 뷰포트에서 바닥 영역이 보이기 시작하면, 해당 영역 + alpha + px 만큼 띄우기