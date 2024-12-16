---

1. 부모 태그의 스타일에 적용해주면 됨

```
display: flex;
```

1. flex 특징
    1. `width: 100%` : 실제로 100%를 차지하는 것이 아니다. px도 마찬가지
    2. 최대한 맞추려고 노력할 뿐
2. 정렬하기

```
justify-content : center;  /* 좌우정렬 */align-items : center;  /* 상하정렬 */flex-direction : column; /* 세로정렬 */flex-wrap : wrap;  /* 폭이 넘치는 요소 wrap 처리 (아래로 내림) */
```

1. 박스 크기를 비율로 설정하기

```
flex-grow: 숫자 (만큼 비율 잡음)
```