---
Create at: 2024-03-22
tags:
  - Javascript
  - Nodejs
  - Socket-io
---
---

#### Socket.io 동작 설계

- 해당 발송기의 전체 메일 할당을 on/off 할때, 이 이벤트를 전역 알림으로 보내야 함.
- 할당 이벤트 명 `assignedTo`
	- 발송기 번호 `agent_no`, 할당할 이메일 명 `email`
- 비할당 이벤트 명 `unassigned`
	- 발송기 번호, 비할당할 이메일 명
- 서버에서 변경 이벤트 발송 `getEmailStateChanged`
	- 발송 주체를 제외하고 보낼때
	- `socket.broadcast.emit`
- 이벤트 2개를 합치는게 낫지 않을까?
	- No. 하나의 이벤트에 하나의 기능만 담자.
	- ~~Yes. 하나의 이벤트에 구분할 수 있는 상태를 담으면 가능.~~
- 이벤트 동작 조건
	- db에 정상적으로 업데이트가 이루어졌을때
	- 