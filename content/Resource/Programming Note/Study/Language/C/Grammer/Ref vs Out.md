---
tags:
  - C
---
---

1. ref 키워드를 이용해서 매개 변수를 넘기는 경우에는 메소드가 해당 매개 변수에 결과를 저장하지 않더라도 컴파일러는 아무런 경고를 하지 않는다.

1. 이와 달리 out 키워드를 이용해서 매개 변수를 넘길 때는 메소드가 해당 매개 변수에 결과를 저장하지 않으면 컴파일러가 에러 메시지를 출력한다.

또한 메소도를 호출하는 쪽에서는 초기화를 하지 않은 지역 변수를  메소드의 out 매개 변수로 넘기는 것이 가능.  
컴파일러가 호출당하는 메소드에서 그 지역 변수를 할당할 것을  보장하기 때문이다.

1. [https://grayt.tistory.com/90](https://grayt.tistory.com/90)