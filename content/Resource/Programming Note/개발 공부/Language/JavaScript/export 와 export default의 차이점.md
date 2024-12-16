# export와 export default의 차이점

JavaScript에서는 모듈을 다른 스크립트에서 사용하기 위해 `export`와 `export default` 키워드를 사용합니다. 이 두 개의 키워드는 유사하지만, 차이점이 있습니다.

## export와 export default란?

- `export`: 모듈에서 특정 변수, 함수, 클래스를 내보낼 때 사용합니다.
- `export default`: 모듈에서 기본으로 내보내는 값을 설정합니다.

## export와 export default의 차이점

- `export`는 이름 있는 내보내기(named exports)이고 `export default`는 이름 없는 내보내기(default exports)입니다.
- `export`는 여러 개의 변수, 함수, 클래스를 내보낼 수 있습니다. `export default`는 모듈에서 하나의 값을 내보낼 수 있습니다.
- `export`된 변수, 함수, 클래스는 이름이 있으므로 중괄호(`{}`) 안에 이름을 명시하여 가져와야 합니다. `export default`된 값은 중괄호 없이 가져올 수 있습니다.

## 예시

```
// profile.jsexport const name = "John";export function getAge() {  return 30;}export class Person {  constructor(name, age) {    this.name = name;    this.age = age;  }}// index.jsimport { name, getAge, Person } from './profile.js'; // 이름 있는 내보내기import user from './profile.js'; // 이름 없는 내보내기
```

위의 예시에서 `profile.js` 모듈에서 `export` 키워드를 사용하여 여러 개의 변수, 함수, 클래스를 내보냈습니다. `index.js`에서는 `import` 키워드를 사용하여 이름 있는 내보내기와 이름 없는 내보내기를 가져왔습니다. 이름 있는 내보내기는 중괄호 안에 가져올 내용을 명시하여 가져왔고, 이름 없는 내보내기는 `user` 변수에 값을 할당하여 가져왔습니다.

## 결론

`export`와 `export default`는 모듈에서 값을 내보내는 방법입니다. `export`는 여러 개의 변수, 함수, 클래스를 내보낼 수 있고, 이름 있는 내보내기입니다. `export default`는 모듈에서 하나의 값을 내보낼 수 있고, 이름 없는 내보내기입니다.