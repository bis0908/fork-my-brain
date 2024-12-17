---
tags:
  - css
---


1. 뼈대 css 와 살점 css 분리하기


#### as-is
```css
.main-btn-red {  
	font-size : 20px;  
	padding : 15px;  
	border : none;  
	cursor : pointer;  
	background : red;
}
.main-btn-blue {  
	font-size : 20px;  
	padding : 15px;  
	border : none;  
	cursor : pointer;  
	background : blue;
}
```

#### to-be
```css
.main-btn {  
	font-size : 20px;  
	padding : 15px;  
	border : none;  
	cursor : pointer;
}
.bg-red {  
	background : red;
}
.bg-blue { 
	background : blue;
}
```

#### BEM?
1. BEM은 뭐임

	1. Block Element Modifier라고 class 작명하는 방법임

```css
class = "덩어리이름__역할--세부특징"
```