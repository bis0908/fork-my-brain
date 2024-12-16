---

> [!info] SLF4J 설정  
> logging 관련 라이브러리는 다양하다. 이러한 라이브러리들을 하나의 통일된 방식으로 사용할 수 있는 방법을 SLF4J는 제공한다. SLF4J는 로깅 Facade이다. 로깅에 대한 추상 레이어를 제공하는 것이고 interface의 모음이다. 참고로 logback-classic 1.2.3은 이미 slf4j-api 1.7.25에 대한 의존성을 가지고 있기 때문에 slf-j-api를 추가할 필요는 없다. Spring은 기본적으로 아파치 재단의 commons-logging을 사용한다. logback라이브러리를 사용하려면 commons-logging를 제거 해야한다.  
> [https://velog.io/@oyeon/SLF4J-%EC%84%A4%EC%A0%95](https://velog.io/@oyeon/SLF4J-%EC%84%A4%EC%A0%95)