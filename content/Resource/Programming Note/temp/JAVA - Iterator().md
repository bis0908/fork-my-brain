  

2022년 4월 18일 월요일

오후 1:30

1. Iterator는 자바의 컬렉션 프레임웍에서 컬렉션에 저장되어 있는 요소들을 읽어오는 방법을 표준화 하였는데 그 중 하나가 Iterator이다.
2. Iterator는 인터페이스다.

public interface Iterator {

boolean hasNext();

Object next();

void remove();

}

1. boolean hasNext() 메소드는 읽어 올 요소가 남아있는지 확인하는 메소드이다. 있으면 true, 없으면 false를 반환한다.
2. Object next() 메소드는 읽어 올 요소가 남아있는지 확인하는 메소드이다. 있으면 true, 없으면 false를 반환한다.
3. void remove() 메소드는 next()로 읽어 온 요소를 삭제한다. next() 를 호출한 다음에 remove() 를 호출해야 한다. (선택적 기능이라 사용해도 그만 사용하지 않아도 그만이다)
4. Iterator는 다시 말해 인터페이스이다. 그렇다면 저 메소드들은 어떻게 정의가 되어있단 말인가?

List 혹은 Set 인터페이스를 구현하는 컬렉션은 iterator() 가 컬렉션의 특징에 맞게 설계가 되어있다