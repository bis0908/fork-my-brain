---
tags:
  - Html
---


1. 모든 `<div>` 태그는 `display: block` 속성을 기본으로 가지고 있다.
    1. 하나의 태그가 가로행을 전부 차지한다.
2. 그래서 이것을 방지하기 위해 `float` 속성을 주고 띄워준다.
    1. 수평적 배치가 아닌 수직으로 배치해 겹치지 않게? 도와줌
    2. `float` 을 사용하고나면 꼭 `clear` 속성이 필요한 것을 잊지말자.
3. background에 색을 주면 `<div>` 영역 알아보기 쉽다

### html
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">    
	<meta name="viewport" content="width=device-width, initial-scale=1.0">    <title>Document</title>    
	<link rel="stylesheet" href="../css/main.css">
</head>
<body>    
<!-- wrapper or container 라고 부르는 제일 큰 박스 하나 구성 -->    
<!-- div 태그는 모두 display: block (가로행 모두 차지) 속성을 이미 갖고 있음 -->    
	<div class="container">        
		<div class="header"></div>
		<div class="left-menu"></div>
		<div class="right"></div>
		<div class="footer"></div>
	</div>
</body>
</html>
```

### css
```css
.container {  
	width: 800px;
	}
.header {  
	width: 100%;  
	height: 50px;  
	background: aquamarine;
}
.left-menu {  
	/* %: 부모 tag에서의 비율 */  
	width: 20%;  
	height: 400px;  
	background: cornflowerblue;  
	/* float: 요소를 붕 띄워준다(다른 요소의 앞으로 가져온다고 생각하면 됨 */  
	float: left;
}
.right { 
	width: 80%;  
	height: 400px;  
	background-color: coral;  
	float: right;
}
.footer { 
	width: 100%;  
	height: 100px;  
	background-color: grey;  
	/* clear: 부유시킨 이후의 문서 흐름을 제거하기 위해 사용됨.  
	left, right, both 각기 방향으로 부유하는 걸 제거해주는 3개 속성이 있다. */  
	clear: both;
}
```

1. 하지만 `display: inline-block` 속성은 부가 설정이 귀찮음.
    1. 얘가 머하는 애냐면 `float` 속성과 유사한 속성인데 띄우는 것 대신 `가로행` 에서  
        본인의 사이즈 만큼만 공간을 가져가 서로 다른 `<div>` 태그를 코드상에서 딱 붙여주면 웹에서도 붙은 것 처럼 보임.
    2. 문제
        1. 이때 div 안에 글을 쓰면 레이아웃이 아작남. 그래서 부모 태그에  
            `font-size: 0px` 를 넣어주고 다시 자식 태그에서 폰트 사이즈 재지정 필요함.
        2. 더해서 `<p>` 태그 써주는거 잊지 말고 !
        3. 더해서22 `vertical-align: top` 잊지 말고 !
2. `baseline` 이 존재하면 `display: inline-block` 요소들이 `baseline` 위에 오려고 하는 문제 있음.

  

### `<div>` 와 속성은 같지만 명시적으로 이름만 다른 태그들

1. `<nav>`
2. `<section>`
3. `<footer>`