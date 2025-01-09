---
tags:
  - C
---


> [!info] [C#] Lazy 소개 및 Singleton 패턴  
> Lazy 는 lazy initialization 기능 을 제공한다. 즉, 선언 즉시 생성이 아니라, 접근(access by Value 프로퍼티)하려 하면 그 때 생성 시켜 주는 것이다. 생성에 시간이 오래 걸리는 큰 오브젝트를 필요할 때만 생성 : 프로그램 구동 속도 저하 방지 자원 집약적인, 즉 리소스를 많이 사용하는 실행을 필요할 때만 자원 생성을 멀티쓰레드 환경에서 안전하게 해야 할 때 : 멀티쓰레드 싱글톤의 경우에 해당 나머지 녀석들은 명확한데, Lazy (LazyThreadSafeMode.PublicationOnly) 는 조금 들여다 봐야 한다.  
> [http://egloos.zum.com/sweeper/v/3157853](http://egloos.zum.com/sweeper/v/3157853)