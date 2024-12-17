---
Create at: 2024-10-08
tags:
  - Javascript
  - dataTables
---
---


```js

columns: [  
  { data: null }, // target
  // ...
],
drawCallback: function (settings) {  
  const api = this.api();  
  const start = api.page.info().start;  
  api.column(0, { order: "applied", search: "applied" }).nodes().each((cell, i) => {  
    cell.innerHTML = api.rows().count() - (start + i);  
  });  
}
```