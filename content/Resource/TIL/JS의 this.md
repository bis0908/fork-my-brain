**화살표 함수 내부의 this는 initComplete 이외의 컨텍스트를 참조**하게 됩니다.

그래서 this.api()가 호출되면, this는 DataTable 인스턴스를 참조하지 않고,

아마도 전역 객체(브라우저에서는 window)나 undefined를 참조하게 될 것입니다.

  

이 문제를 해결하기 위해, function 키워드를 사용하여 함수를 선언해 보세요. ￼**function 키워드는 자신만의 this 바인딩을 생성**하므로

this는 DataTable 인스턴스를 참조할 것입니다.