---

> [!info] jQuery : val(), text(), html()의 차이  
> 참고 : http://api.jquery.com ★ val() : Form Element 의 값을 받아오는데 쓰인다. (주로 input 이나 textarea 정도?) - 주의해야할 점은 Form Element 이외의 값은 받아오질 못한다는 점. ★ val(value) : value의 경우 string 또는 string의 배열(이 경우 value들의 matching을 잘 시켜야 오류를 피할 수 있다) 또는 함수(function(index, value) 이런 형태)로 넣을 수 있다. 이 함수 역시 Form Element의 Value 값을 Set할 때 주로 쓰인다. ● text() : XML과 HTML 문서에서 둘다 사용될 수 있다. input elements 의 value를 받아오지 못한다(이 경우 val을..  
> [https://secretroute.tistory.com/entry/jQuery-val-text-html의-차이](https://secretroute.tistory.com/entry/jQuery-val-text-html의-차이)