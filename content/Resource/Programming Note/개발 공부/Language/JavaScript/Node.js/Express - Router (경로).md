---

- router 사용하는 법을 몰라 링크 내용을 보며 학습했다

> [!info] [Node.js] express router 사용하기  
> node.js의 Restful API들을 모두 한 js 파일에 작성하게 되면 코드를 알아보기가 점점 힘들어진다. 그렇기 때문에 토픽에 맞는 router를 생성해서 path를 지정하면 각 js별로 토픽이 정해져 있어 유지 보수가 더 간단해지고 팀원들 간의 코드 리뷰에도 큰 도움이 된다. 시작하기에 앞서 지난 시간에 포스팅한 코드를 사용할 것이므로 만약 이 포스팅에 먼저 들어왔다면 여기로 가서 지난 포스팅을 간략하게 읽고 오길 바란다.  
> [https://millo-l.github.io/Nodejs-express-router-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0/](https://millo-l.github.io/Nodejs-express-router-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0/)  

1. Router를 사용하는 이유
    1. API를 유형별로 관리하기 위해서.
    2. 하나의 파일에 다 때려박는 건 좋지 않은 습관.
2. 예제 (이메일 주소를 입력하면 하단의 `<div>` 에 이메일 주소를 back에서 던져주는 함수)
    1. 굵게 표시된 코드가 핵심

```
const express = require("express");const app = express();const bodyParser = require("body-parser");const path = require("path");const PORT = 3000;const scrapRouter = require("./routes/scrap");app.use(bodyParser.urlencoded({ extended: true }));app.use("/public", express.static(__dirname + "/public"));app.use("/node_modules", express.static(__dirname + "/node_modules"));app.set("view engine", "ejs");app.use("/ajax", scrapRouter);app.listen(PORT, () => {  console.log(`listening on ${PORT}`);  console.log(`http://localhost:${PORT}`);});console.log("app.use: scrapRouter");app.get("/", (req, res) => {  try {    res.render("index.ejs");    console.log("loading index.ejs");  } catch (e) {    console.log(e.message);  }});// app.post("/ajax", (req, res) => {//   let email = req.body.email;//   let resData = {};//   resData.email = email;//   res.json(resData);// });
```

```
const express = require("express");const router = express.Router();console.log("Entering scrap routes");router.get("/", (req, res) => {  //   let email = req.body.email;  // Send Post  let email = req.query.email; // Send Get  let resData = {};  resData.email = email;  console.log(resData);  res.json(resData);});module.exports = router;
```

GET, POST일때 req에서 받는 형태가 다른걸 주의하자

```
<!DOCTYPE html><html lang="en"><head>    <meta charset="UTF-8">    <meta http-equiv="X-UA-Compatible" content="IE=edge">    <meta name="viewport" content="width=device-width, initial-scale=1.0">    <link rel="stylesheet" href="/public/stylesheets/style.css">    <script type="text/javascript" src="/node_modules/jquery/dist/jquery.min.js"></script>    <title>main</title>    <!--    https://blog.naver.com/king2992/221848404700    --></head><body>    <h1>        Hello World!    </h1>    <form action="/ajax" method="get">        Email: <input type="text" name="email">        <br>        <br>    </form>    <button class="sendAjax">        send_Ajax    </button>    <div class="result"></div>    <script>        $(document).ready(() => {            $(".sendAjax").on("click", () => {                // let email = $('input[name=email]').val();                let email = $('input').val();                $.ajax({                    url: '/ajax',                    type: 'get',                    dataType: 'json',                    data: { 'email': email }                })                    .done((json) => {                        $('.result').text(json.email)                    })                    // .fail(function (xhr, status, errorThrown) {                    .fail((xhr, status, errorThrown) => {                        alert('Ajax failed')                    })            });        })    </script>    <!-- <script src="https://code.jquery.com/jquery-3.6.3.min.js"        integrity="sha256-pvPw+upLPUjgMXY0G+8O0xUf+/Im1MZjXxxgOcBQBXU=" crossorigin="anonymous"></script> --></body></html>
```

  

### 2023.03.07 추가 정리

---

`app.use()` 로 하루를 뺑이치면서 다시 배운 것 정리.

1. 상황

- 외부에서 추가되는 bootstrap, jquery를 `npm install` 로 `node_modules` 에 설치한 다음
- `routes` 폴더에서 `vendors.js` 에 라우터를 지정해주고 최종적으로 `index.ejs` 내부에서 외부 소스 태그만 빼놓은 `vendors.ejs` 를 포함시키는 상황.
- `app.js` 에서는 아래와 같이 사용했다.

```
import { vendorsRouter } from "./routes/vendors.js";(...)app.use("/vendors", vendorsRouter);
```

- html에서 `script` 를 불러오지 못한 **결정적인 원인**

```
import express from "express";import path from "path";export const vendorsRouter = express.Router();const __dirname = path.resolve(); // for ES modulevendorsRouter.use(  "/bootstrap",  express.static(path.join(__dirname, "../node_modules/bootstrap/dist")));
```

- `__dirname` 과 결합되는 경로 앞에 상위 폴더를 의미하는 `../` 이 붙어있던 것이 원인이었다.
- 여기서 절대경로와 결합되지 못한게 1차 원인이었고
- 두번째 원인

```
<script src="/vendors/bootstrap/js/bootstrap.min.js"></script>
```

- ejs 파일에서 최종 표현되는 `/vendors/bootstrap` 의 경로가 무엇을 의미하는지 앞의 코드를 제대로 이해하지 못했었다.
- 이미 앞의 `app.js`에서 `/vendors` 의 경로를 라우터로 분리한 것을 알려주었었고,
- `/bootstrap` 의 경로는 `vendors.js` 내부에서 `.use` method로 지정을 한 것을 인지하지 못한게 원인이었다.
- 위 코드를 이해하고 제대로 경로를 지정해주니 `index.ejs` 파일에서 제대로 인식했다.