---
tags:
  - css
---


## DOM이란 무엇인가

1. 문서 객체 모델

> [!info] DOM 소개 - Web API | MDN  
> 이 문서는 DOM에 대한 개념을 간략하게 소개하는 문서이다: DOM 이 무엇이며, 그것이 어떻게 HTML, 문서들을 위한 구조를 제공하는지, 어떻게 DOM 에 접근하는지, API 가 어떻게 사용되는지에 대한 참조 정보와 예제들을 제공한다. 문서 객체 모델(The Document Object Model, 이하 DOM) 은 HTML, XML 문서의 프로그래밍 interface 이다.  
> [https://developer.mozilla.org/ko/docs/Web/API/Document_Object_Model/Introduction](https://developer.mozilla.org/ko/docs/Web/API/Document_Object_Model/Introduction)  

1. 문서 객체 모델(The Document Object Model, 이하 DOM) 은 HTML, XML 문서의 프로그래밍 interface 이다. DOM은 문서의 구조화된 표현(structured representation)을 제공하며 프로그래밍 언어가 DOM 구조에 접근할 수 있는 방법을 제공하여 그들이 문서 구조, 스타일, 내용 등을 변경할 수 있게 돕는다. DOM 은 nodes와 objects로 문서를 표현한다. 이들은 웹 페이지를 스크립트 또는 프로그래밍 언어들에서 사용될 수 있게 연결시켜주는 역할을 담당한다.
2. Shadow DOM은 DOM 내부에 숨어있는 태그들이라고 봐도 될거 같다.
3. 예를들어 `<input>` 태그 입력시 버튼과 텍스트가 동시에 표시되는데 이런 숨어진 태그들의 집합이 바로 Shadow DOM.
4. 마찬가지로 얘네들도 커스터마이징 가능하다

```
appearance: none;
```

```
progress {  appearance: none;}progress::-webkit-progress-bar {  background: \#f0f0f0;  border-radius: 10px;  box-shadow: inset 3px 3px 10px \#ccc;}progress::-webkit-progress-value {  border-radius: 10px;  background: \#1d976c;  background: -webkit-linear-gradient(to right, \#93f9b9, #1d976c);  background: linear-gradient(to right, #93f9b9, #1d976c);}
```