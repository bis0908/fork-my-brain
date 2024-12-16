---
Create at: 2023-12-04
tags:
  - Javascript
---
- `.forEach()` 는 비동기를 기다려주지 않는다.
- `for await ...of`를 사용하면 된다.
```js
const tableRow = $(".Ltbl_list_lvl0, .Ltbl_list_lvl1"); // cheerio Object

for await (const row of tableRow) {
    const [_, saNo, maemulSer] = $(row)
      .find("input[type=checkbox]")
      .val()
      .split(",");

    const courtAndCase = removeTabAndLineBreak($, $(row).find("td").eq(1));
    const [court, ...case_number] = courtAndCase;
    console.log(
      "🔥 / file: search-service.js:196 / $ / case_number:",
      case_number
    );
    
    // ...
```

- `for of(), for in()` 도 가능하다.
```js
for (const param of params) {
  const res = await axios.get(`https://localhost/?id=${param}`);
  resArray.push(res.data);
}

for (const index in params) {
  const res = await axios.get(`https://localhost/?id=${params[index]}`);
  resArray.push(res.data);
}

```