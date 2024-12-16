---

강의에서는

```
res.sendFile(__dirname + ‘index.html’)
```

으로 하라고 되어있으나, 만약 html 파일이 다른 상대 경로에 있을경우 (나의 경우는 당시에 아래와 같은 폴더 구조였다.

```
project_name> node_modules> public	> css	> js		- server.js	index.html> routes> viewspackage.jsonpackage-lock.json
```

아래와 같이 수정하면 접근 가능해진다.

```
res.sendFile('index.html', {root: 'public'});
```