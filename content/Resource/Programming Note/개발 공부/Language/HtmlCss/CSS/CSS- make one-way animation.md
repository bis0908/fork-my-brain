---

## ➡️한방향으로 정지 없이 이동하는 애니메이션을 의미.

1. 시작스타일 정하기
2. 최종스타일 정하기
3. 언제 최종스타일로 변할지 트리거 주기 (대부분 마우스 올렸을 때임)
4. transition 으로 서서히 동작하게 만들기

### transition 속성

```
.someClass { transition: all 1s}
```

- transition 속성이 부여되면 여기 적용된 css가 변할때 천천히 변경되게 해줌.
    - all: 모든 스타일이 변할때
    - 혹은 특정 속성 하나만

```
.someClass {  transition-delay: 1s; /* 시작 전 딜레이 */  transition-duration: 0.5s; /* transition 작동 속도 */  transition-property: opacity; /* 어떤 속성에 transition 입힐건지 */  transition-timing-function: ease-in; /* 동작 속도 그래프조정 */}
```

## 이미지 위에 오버레이 하기

1. 빈 투명박스 `<div>` 를 만들어놓기
2. 자식 `<div>` 태그에 덮을 박스 하나 만들기 (여기서 배경색 지정)

```
<div style="position: relative; overflow: hidden">  <div class="overlay">        <div class="overlay-black">            <span>$60</span>        </div>    </div>    <img src="../img/product1.jpg" alt="product1"></div>
```

```
.overlay {  position: absolute;  width: 100%;  height: 100%;  color: whitesmoke; /* font color */}.overlay-black {  position: absolute;  width: inherit;  height: inherit;  top: 100%;  background-color: rgba(0, 0, 0, 0.5);  opacity: 0;  transition: all 1s;  color: inherit;}.overlay:hover .overlay-black {  top: 50%;  opacity: 1;}
```