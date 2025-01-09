---
tags:
  - Nodejs
  - debug
---
- Case 1: vscode에서 nodejs 디버깅을 하려는 상황.
- 시작점은 `${workspaceFolder}/views/index.ejs` 이고 디버깅 지점은 `${workspaceFolder}/common/globalFunction.js` 임.
- 이때 launch.json 의 가장 적절한 webRoot 경로는 어디일까?

```js
"version": "0.2.0", 
  // "configurations": [  
  //   {  
  //     "type": "chrome",  
  //     "request": "launch",    
  //     "name": "localhost에 대해 Chrome 시작",  
  //     "url": "http://localhost:3000",  
  //     "webRoot": "${workspaceFolder}/views/"  
  //   }  
  // ]

```

- 웹 리서치를 참고해 아래와 같은 구성으로 실행해봤더니 성공했다.

```js
"version": "0.2.0",
"configurations": [{
    "type": "node",
    "name": "Debug Node.js",
    "request": "launch",
    "program": "${workspaceFolder}/app.js",
}]
```

- 단, 디버깅시 nodemon 등을 이용한 로컬 구동은 하지 않은 상태로 시작했다.