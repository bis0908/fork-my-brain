---

1. 반응형 웹 만들때 자식 태그 width는 `%` 속성을 자주 사용함.
2. 근데 폭이 엄청 길어질 수 있기 때문에 제한을 줄 수 있음.
3. `width` 는 content 영역의 너비를 의미함. (==**중요**==)
    1. `padding`, `border` 는 `width` 와 상관없음.
    2. 둘 다 포함하라고 시킬 수 있다. `box-sizing: border-box` 속성 적용!

```
div {	box-sizing: border-box;}
```