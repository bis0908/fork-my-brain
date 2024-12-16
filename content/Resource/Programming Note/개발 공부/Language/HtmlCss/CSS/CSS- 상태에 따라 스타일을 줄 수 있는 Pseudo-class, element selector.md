---

### Pseudo-class

> [!info] Pseudo-classes - CSS&colon; Cascading Style Sheets | MDN  
> A CSS pseudo-class is a keyword added to a selector that specifies a special state of the selected element(s). For example, the pseudo-class &colon;hover can be used to select a button when a user's pointer hovers over the button and this selected button can then be styled.  
> [https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes)  

```
.btn:hover {    /*마우스를 올려놓을 때*/  background : chocolate; }.btn:focus {    /*클릭 후 계속 포커스 상태일 때*/  background : red; }.btn:active {   /*클릭 중일 때*/  background : brown; }
```

```
:any-link /*방문 전, 방문 후 링크 한번에 선택할 때*/:playing /*동영상, 음성이 재생중일 때*/:paused /*동영상, 음성이 정지시*/:autofill /*input의 자동채우기 스타일*/:disabled /*disabled된 요소 스타일*/:checked /*체크박스나 라디오버튼 체크되었을 때*/:blank /*input이 비었을 때*/:valid /*이메일 input 등에 이메일 형식이 맞을 경우*/:invalid /*이메일 input 등에 이메일 형식이 맞지 않을 경우*/:required /*필수로 입력해야할 input의 스타일*/:nth-child(n) /*n번째 자식 선택*/:first-child /*첫째 자식 선택*/:last-child /*마지막 자식 선택*/
```

### pseudo-element

> [!info] ::after (:after) - CSS: Cascading Style Sheets | MDN  
> 다음 예제는 ::after와 함께 CSS 표현식, data-descr 사용자 설정 데이터 속성 을 사용해 툴팁을 구현합니다. JavaScript 없이요! tabindex="0"을 추가해 각 span에 포커스가 갈 수 있도록 지정한 후, CSS :focus 선택자를 추가하여 키보드 사용자도 지원할 수 있습니다. 예제를 통해 ::before와 ::after가 얼마나 유연한지 확인할 수 있지만, 가장 접근성이 뛰어난 구현을 위해서라면 요약과 세부 요소 처럼 의미를 담은 요소를 활용하는 편이 좋습니다.  
> [https://developer.mozilla.org/ko/docs/Web/CSS/::after](https://developer.mozilla.org/ko/docs/Web/CSS/::after)  

```
.text::first-letter {  color : red;}.text::first-line {  color : red;}.text::after {  content : '뻥이지롱';  color : red;}.text::before {  content : '뻥이지롱';  color : red;}
```