  

2022년 4월 7일 목요일  
오후 2:31

자바스크립트의 유연한 특성 때문에 작성이 편한 반면, 파일이 많아지면 생산성이 떨어진다는 점 때문에 타입스크립트를 많이 사용하는데, 반드시 자바스크립트를 이용해 앱을 개발해야 하는 상황에서는 이러한 문제점을 피하기 위해 PropTypes를 활용하는 것을 권장한다.

PropTypes는 부모로부터 전달받은 prop의 데이터 type을 검사한다. 자식 컴포넌트에서 명시해 놓은 데이터 타입과 부모로부터 넘겨받은 데이터 타입이 일치하지 않으면 콘솔에 에러 경고문이 띄워진다. 간단한 예시를 통해 propTypes의 장점을 알 수 있다.

import PropTypes from 'prop-types';

Greeting.propTypes = {  
name: PropTypes.string  
};

const Greeting = ({ name }) => {  
return (  
<h1>Hello, {name}</h1>  
);  
}  
위의 코드에서 부모에게서 받아온 name이라는 props가 string 형태가 아니라 숫자, 함수 또는 객체 등 다른 형태면 콘솔에 에러 메시지가 출력된다. 이는 리액트가 렌더링하는 과정에서 잘못된 속성값 타입을 검사해 주기 때문에 가능한다. 물론 속성값 타입을 검사하기 위해 별도의 연산이 필요하다. 따라서 성능상의 이유로 개발 모드에서만 확인된다.

const User = ({ type, age, male, onChangeName, onChangeTitle }) => {

const onClick1 = () => {  
const msg = `type: ${type}, age: ${age ? age : '알 수 없음'}`  
log(msg, { color: type === "gold" ? "red" : "black" })  
// ...  
}

const onClick2 = () => {  
if (onChangeName) onChangeName(name)  
onChangeTitle(title)  
male ? goMalePage() : goFemalePage()  
// ...  
}  
// ...  
}  
PropTypes를 사용함으로써 생기는 또 다른 장점은 타입 정의만으로도 좋은 문서가 될 수 있다는 점이다. 위의 코드는 컴포넌트를 사용하는 사람이 자세히 뜯어보지 않으면 속성값의 타입을 알기가 어렵다. 하지만 propTypes를 이용해 속성값의 타입 정보를 위에 정의해 주면 컴포넌트의 로직을 이해하지 않고도 타입 정보를 한눈에 파악할 수 있다.

User.propTypes = {  
male: PropTypes.bool.isRequired,  
age: PropTypes.number,  
type: PropTypes.oneOf(["gold", "silver", "bronze"]),  
onChangeName: PropTypes.func,  
onChangeTitle: PropTypes.func.isRequired  
}

const User = ({ type, age, male, onChangeName, onChangeTitle }) => {  
// ...  
male 속성값은 필수 값이기 때문에 부모 컴포넌트에서 이 값을 보내지 않으면 에러 메시지가 출력된다. 반대로 age 속성값은 필숫값이 아니기 때문에 이 값을 주지 않아도 에러는 발생하지 않는다. 그러나 age 속성값에 number가 아닌 타입이 들어오면 에러가 발생한다. Type 속성값으로는 "gold", "silver", "bronze" 중 한 개만 올 수 있다. oneOf 이 인자로 들어가는 배열에 포함된 값 중에서 하나를 만족해야 한다는 의미다. onChangeName, onChangeTitle는 함수 타입이어야 하는데, 함수의 매개변수와 반환값에 대한 타입값까지 여기서 정의할 수는 없다.