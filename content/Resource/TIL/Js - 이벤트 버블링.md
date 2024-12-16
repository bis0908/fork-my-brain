1. 어떤 html 태그에 이벤트가 발생하면 그의 모든 상위 요소까지 이벤트가 실행되는 현상.

```
document
```

.querySelector(

```
'.black-bg'
```

).addEventListener(

```
'click'
```

,

```
function
```

```
(e)
```

{

e.target;

e.currentTarget;

e.preventDefault();

e.stopPropagation();

})