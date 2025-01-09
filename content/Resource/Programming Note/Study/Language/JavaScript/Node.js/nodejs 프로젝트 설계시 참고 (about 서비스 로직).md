---
tags:
  - Nodejs
---


> [!info] [node.js] 견고한 node.js 프로젝트 설계하기 (번역)  
> 본 포스트는 Sam Quinn의 “Bulletproof node.js project architecture” 글을 번역한 것입니다. 혼자 보기 너무 아까운 글이기에 번역하여 공유합니다. Introduction Express.js는 node.js REST API를 만드는데 좋은 프레임워크지만, 어떻게 node.js 프로젝트 구조를 잡아야 할지 알려주지 않습니다. 우습게 들릴 수도 있지만, 이건 매우 큰 문제입니다. 올바른 node.js 프로젝트 구조는 코드의 중복을 피해 주고 안정성을 높여주며, 당신의 서비스를 확장하는데 도움이 될 것입니다. 이 포스트는 다년간의 부족했던 설계와 나쁜 패턴, 그리고 수없이 많은 코드 리팩터링 경험을 통해 쓰인 하나의 리서치입니다. 목차 폴더 구조 3 계층 설계 (3 Lay..  
> [https://charming-kyu.tistory.com/16](https://charming-kyu.tistory.com/16)  

  

> [!info] NodeJS - 로직 분리(routes / models / controllers)  
> (/routes/post.js) : routes에서 들어오는 api요청에 대해서 라우팅을 하면서 동시에 비즈니스 로직을 작성했음      \--> 가독성 저하 + 유지보수 비 용이           \--> 비즈니스 로직을 처리하는 controller를 만들어 분리  
> [https://velog.io/@neity16/NodeJS-로직-분리routesmodels-controllers](https://velog.io/@neity16/NodeJS-로직-분리routesmodels-controllers)  

  

> [!info] [Node Express] 서버에 3 Layer Architecture 적용하기  
> 🍔 Intro. 아래 코드는 3 Layer Architecture를 적용하지 않고 모두 route 폴더에 작성한 예시입니다. 이렇게 작성하면 기능이 복잡해질 때, 한 파일의 코드가 너무 길어지고, 가독성도 떨어집니다. 또, layer나 모듈로 분리하지 않아서 여러 파  
> [https://velog.io/@ju_h2/Node-express-서버에-3-Layer-Architecture-적용하기](https://velog.io/@ju_h2/Node-express-서버에-3-Layer-Architecture-적용하기)