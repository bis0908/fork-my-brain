---

> [!info] 여씨의 개발이야기  
> 위와 같이 이미지와 텍스트가 주어져있을 때 이미지 위에 텍스트를 입히는 방법을 알아보고자 한다. 1. wrap, image, text div를 구성해준다. {id}님의 덕담보따리 2. 각 div에 맞춰 css를 넣어준다. .user-wrap { width: 100%; margin: 10px auto; position: relative; } .user-wrap img { width: 100%; vertical-align: middle; } .user-text { position: absolute; top: 40%; left: 50%; width: 100%; transform: translate( -50%, -50% ); font-size: 20px; font-family: 'ypseo'; text-alig..  
> [https://yeoossi.tistory.com/28](https://yeoossi.tistory.com/28)  

1. 이미지와 텍스트 태그 상위 컨테이너에 `positon: relative` 속성 추가
2. text 요소에 `position: absolute` 추가
    1. 나의 경우 `top: 0` 을 넣어줘야 이미지 위에 떴었음
3. magin을 조정해 적절히 위치를 조정한다.