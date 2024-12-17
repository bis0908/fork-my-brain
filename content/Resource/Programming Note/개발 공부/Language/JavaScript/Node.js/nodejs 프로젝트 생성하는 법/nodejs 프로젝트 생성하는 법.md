---
tags:
  - Nodejs
---


## 쌩 처음부터 프로젝트 구축 참고용

```
npm i -g express-generator
```

- 프로젝트 환경 구성하기

```
express "pjt-name" --view=ejsorexpress ./ --view=ejs
```

- 아래와 같은 구조가 생성된다.
    
    ![[/Untitled 4.png|Untitled 4.png]]
    

  

- Express 폴더 구조 뜯어보기
    
    > [!info] [Node.js] \#6 Express / Express-generator로 프로젝트 만들기  
    > 이 노트는 "Node.js 교과서"를 공부하면서 기록되었다. Node.js로 http 모듈을 사용해 웹서버에 필요한 기능들을 하나하나 다 구현하고 있으면 정말 복잡하고 귀찮은 일이 될 것이다. 이것을 간단하게 처리해주는 것이 Express! Express.js는 지난 편에 정리한 HTTP 요청에 대해 라우팅과 미들웨어 기능을 제공하는 웹 프레임 워크이다. 사실 Node.js 안에도 Express가 아닌 다른 웹 서버 프레임 워크들이 존재한다.  
    > [https://velog.io/@new_wisdom/Node.js-6-Express-Express-generator%EB%A1%9C-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0](https://velog.io/@new_wisdom/Node.js-6-Express-Express-generator%EB%A1%9C-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0)  
    

  

- 이제 필요한 라이브러리를 npm으로 설치해주면 된다.
- 일반적으로 사용하는 라이브러리들
    
    - 참고사항임
    
    > [!info] 꼭 알아야할 NodeJs 라이브러리들  
    > Backend 라이브러리 Express 빠르고 가벼운 웹 프레임워크 Socket.io 실시간 양방향 이벤트 기반 커뮤니케이션 Body-parser Request body 파싱 미들웨어 Cors CORS 허용을 위한 Connect/Express 미들웨어 Passport Express 호환 인증 미들웨어 알려진 전략들의 확장 가능한 세트 Multer multipart/form-data 제어를 위한 미들웨어 주로 파일 업로드에서 사용.  
    > [https://corine2.tistory.com/304](https://corine2.tistory.com/304)  
    
    moment.js(Date 라이브러리)는 Deprecated 되었다고 하니 참고
    
    Luxon이 대체가 될 수도?
    

[[express 핵심 기초 정리]]