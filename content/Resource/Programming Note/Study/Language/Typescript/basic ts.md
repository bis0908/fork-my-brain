---
Create at: 2024-08-07
tags:
  - typescript
---
---

📌 Types of TS(기본)  
✅ 배열: `자료형[]` 
✅ 숫자: `number` 
✅ 문자열: `string` 
✅ 논리: `boolean` 
✅ optional  
```typescript
const player : {  
    name: string,  
    age?:number // ?: optional } = {  
    name: "nico"  
	}  
```

  
```typescript
// error!
if (player.age < 10) {  

}

// 존재하는지 체크가 필요
if (player.age && player.age < 10) {  
}
```
  

```
  
  
❗ ?를 :앞에 붙이면 optional  
  
✅ Alias(별칭) 타입  
```typescript
type Player = {  
    name: string,  
    age?:number  
}  
  
const player : Player = {  
    name: "nico"  
}  
```

  
⭐ 함수에서는 어떻게 쓸까  
```typescript
type Player = {  
    name: string,  
    age?:number  
}  
  
function playerMaker1(name:string) : Player {  
    return {  
        name  
    }  
}  
  
const playerMaker2 = (name:string) : Player => ({name})  
  
const nico = playerMaker1("nico")  
nico.age = 12
```

✅ readonly 사용하기
```typescript
type Player = {  
    readonly name:string  
    age?:number  
}  
  
const playerMaker = (name: string): Player => ({name})  
  
const nico = playerMaker("nico")  
//🚫 nico.name = "aa"  

const numbers: readonly number[] = [1, 2, 3, 4]  
//🚫 numbers.push(1)  
//❗ readonly가 있으면 최초 선언 후 수정 불가  
//    ⇒ immutability(불변성) 부여  
//        but, javascript에서는 그냥 배열  

```


✅ Tuple  
정해진 개수와 순서에 따라 배열 선언  
```typescript
const player: [string, number, boolean] = ["nico", 1, true]  
//❗ readonly도 사용가능 ⇒ readonly [...] 형태  
```

  
✅ undefined, null, any  
any: 아무 타입  
undefined: 선언X 할당X  
null: 선언O 할당X

---

##### Class, 접근 제한자 Practice
```typescript
type Words = {
  [key: string]: string;
};

class Dict {
  private words: Words;
  constructor() {
    this.words = {};
  }
  add(word: Word) {
    if (this.words[word.term] === undefined) {
      this.words[word.term] = word.def;
    }
  }
  def(term: string) {
    return this.words[term];
  }
  update(word: Word) {
    if (this.words[word.term] !== undefined) {
      this.words[word.term] = word.def;
    }
  }
  del(term: string) {
    if (this.words[term] !== undefined) {
      delete this.words[term];
    }
  }
}

class Word {
  constructor(
    public term: string,
    public def: string,
  ) {}
}

const kimchi = new Word("kimchi", "super cool food");
const pizza = new Word("pizza", "super nice piazza");
const dict = new Dict();

dict.add(kimchi);
dict.add(pizza);
console.log("KIMCHI:", dict.def("kimchi"));
console.log("PIZZA:", dict.def("pizza"));

dict.update(new Word("kimchi", "very incredible super food"));
console.log("UPDATE KIMCHI:", dict.def("kimchi"));
console.log("NOT UPDATE PIZZA:", dict.def("pizza"));

dict.del("pizza");
console.log("DELETE PIZZA", dict.def("pizza"));
console.log("NOT DELETE KIMCHI:", dict.def("kimchi"));
```