---
tags:
  - Nodejs
  - Unity
---


1. Keyword: Node Redis
2. Mset mget: multi get set
3. Keyworkd: node js websocket(npm official guide)
4. Websocket 알아둬야할 것
    1. MDN 문서 참조
    2. Open, message, error, close
5. Json: key list
6. Adapter: Redis로부터 keylist를 받는다
7. Redis 에서 keylist로 mget
8. Mget의 결과를 유니티 WebGL로 전송(WebSocket을 통해) -> 반복문
    1. 키, 값 쌍으로 가져와 보낸다
9. Json: key Val list
10. 유니티로부터 Key:Val set을 받는다.
11. Redis에 Key:Val set을 mset 수행