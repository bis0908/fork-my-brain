---
Create at: 2024-10-08
tags:
  - Javascript
  - dataTables
---
---


```js
// DataTable을 생성하는 코드 내부
columns: [  
  { data: null }, // target (아래 drawCallback에서 생성)
  // ... other columns
],
drawCallback: function (settings) {  
  const api = this.api();  
  const start = api.page.info().start;  
  api.column(0, { order: "applied", search: "applied" }).nodes().each((cell, i) => {  
    cell.innerHTML = api.rows().count() - (start + i);  
  });  
}
```