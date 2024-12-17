---
tags:
  - Html
---


```
.cart-table td:nth-child(2) {  color: red;}
```

- 여러 요소를 찾은 다음 원하는 n번째 요소만 스타일을 주고 싶으면 :nth-child(n) 이걸 뒤에 붙여주면 됩니다.
- 원하는 n번째 요소만 스타일을 주고 싶으면 :nth-child(n) 이걸 뒤에 붙여주면 됩니다.
- 위의 코드는 그래서 .cart-table 안에 있는 모든 td를 찾은 다음 2번째 나오는 td에만 color를 줄 수 있습니다.
- 테이블에서 원하는 순서의 셀에 스타일줄 때 가끔 유용하게 사용합니다.

  

```
.cart-table td:nth-child(even) {  color: red;}
```

```
.cart-table td:nth-child(3n+0) {  color: red;}
```