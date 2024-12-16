---

> [!info] Viewport 설정이 필요한 이유  
> 요즘 웹사이트는 PC 뿐만아니라 모바일, 태블릿 등 다양한 기기에서 열람 될 수 있다. 그래서 우리는 다양한 기기에서 정상적으로 동작하는 웹사이트를 개발하기 위해서 반응형 웹을 개발한다. 이 반응형 웹의 기초가 되는게 바로 Viewport 설정이라고 한다. 이게 어떤 개념이고 왜 기초라고 하는지 알아보자! Viewport는 현재 Display에 보여지고 있는 영역 을 말한다.  
> [https://velog.io/@huurray/Viewport-%EC%84%A4%EC%A0%95%EC%9D%B4-%ED%95%84%EC%9A%94%ED%95%9C-%EC%9D%B4%EC%9C%A0](https://velog.io/@huurray/Viewport-%EC%84%A4%EC%A0%95%EC%9D%B4-%ED%95%84%EC%9A%94%ED%95%9C-%EC%9D%B4%EC%9C%A0)  

```
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

이 설정에서 width=device-width값을 설정해 주었기 떄문에 전체적인 웹 페이지의 width가 기기의 너비 만큼 구체적으로 설정되고 `@media` 에서 선언된 width의 범위에 따라 css가 적용되어 결과적으로 반응형 웹을 만들 수 있다. (블로그 내용 발췌)