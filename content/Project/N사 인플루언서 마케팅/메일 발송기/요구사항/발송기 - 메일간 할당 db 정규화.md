---
Create at: 2024-03-27
tags:
  - renamailer
  - design/idea
---
---

### 설계
---
![[../../../../../public/Project/ReviewNavi/메일-발송기/attachments/KakaoTalk_20240327_181727106.png]]

1. 테이블은 3가지가 존재
	- 메일만 있는 테이블 (이미 있음)
	- 할당 관리하는 테이블 (신규, **mail_allocation_registry**)
	- 연결될 발송기 테이블 (이미 있음)
2. DB
- 할당 테이블 기준 할당 체크
```sql
select mar.*, ma.name, mgs.id
from mail_allocation_registry mar
join mail_google_sender mgs on mar.mail_no = mgs.no
join mail_agent ma on mar.agent_no = ma.no
where mar.agent_no = '';
```

-  메일 기준 에이전트 할당 체크
```sql
select mar.*, ma.name, mgs.id
from mail_google_sender mgs
join mail_allocation_registry mar on mgs.no = mar.mail_no
join mail_agent ma on mar.agent_no = ma.no
where mgs.id = '';
```

- 에이전트 기준 할당된 메일 가져오기
```sql
select mar.*, ma.name, mgs.id
from mail_agent ma
join mail_allocation_registry mar on ma.no = mar.agent_no
join mail_google_sender mgs on mar.mail_no = mgs.no
where ma.no = '';
```

- insert
```sql
INSERT INTO mail_allocation_registry (mail_no, agent_no) VALUES (?, ?);
```

- delete
```sql
DELETE FROM mail_allocation_registry WHERE agent_no = ?;
```


### 구현
---
1. [x] mail_allocation_registry 테이블 생성 ✅ 2024-03-27
2. [x] insert, delete 쿼리 연결 ✅ 2024-03-27
	1. [x] 프론트 관련 동작 연결 ✅ 2024-03-27
3. [x] 전체할당해제 쿼리 변경 ✅ 2024-03-27
4. [x] A 발송기에 할당된 메일계정 B 발송기에서 선택시 경고 알림 ✅ 2024-03-27
	1. [x] 계속 진행? db insert ✅ 2024-03-27
	2. [x] 취소: no action ✅ 2024-03-27
5. [ ] 발송메일 리스트 - 타 발송기에 메일 할당시 badge 표시
	1. [ ] 개념: agent에 발송할 메일을 추가하거나 뺀다.
	2. [x] 초안: 현재 선택한 발송기명은 무시하고 각 메일별 할당된 agent 이름을 가져온다. ✅ 2024-03-28
	3. [x] 할당 리스트 호출 ✅ 2024-03-28
		1. [x] mar에 할당된 no와 agent 이름을 조합해서 가져온다. ✅ 2024-03-28
		2. [x] 각 순회를 돌면서 data와 일치할때만 badge 추가. ✅ 2024-03-28
	4. [ ] 발송기 선택 여부와 상관없이 할당됐으면 badge 표시
		1. [ ] 시각적 표시가 더 도움됨
	5. [x] 실시간 변경 이벤트 socket으로 받기 ✅ 2024-03-28
		1. [x] 메일 할당 스위치를 on/off 하면 ✅ 2024-03-28
		2. [x] 현재 발송기 이름을 가져온다. ✅ 2024-03-28
		3. [x] 스위치의 data (mailNo(in id), email)를 추출한다. ✅ 2024-03-28
		4. [x] socket으로 { 발송기이름, mail_no, email, agent_no }를 전송한다. ✅ 2024-03-28
			1. [x] 스위치 on 한 메일 계정 옆에 badge를 추가/삭제한다. ✅ 2024-03-28
				1. [x] 로직 ✅ 2024-03-28
					1. [x] 선택한 발송기가 있고, 그 안에서 사용할 메일 할당시 badge를 추가/제거하는 과정임. ✅ 2024-03-28
					2. [x] 리스트의 각 스위치는 mail_google_sender의 번호를 갖고 있음. ✅ 2024-03-28
		5. [x] 전체 할당 해제시 모든 badge 제거 필요 ✅ 2024-03-27
	6. [ ] agent 이름 변경시 badge 이름도 변경 필요.
		1. [x] 발송기 이름이 변경됨 ✅ 2024-03-28
		2. [x] before, after 이름 및 발송기 번호를 가져온다. ✅ 2024-03-28
		3. [x] agent_no와 mail_no는 다름. 그래서 헷갈리는 것임. ✅ 2024-03-28
		4. [x] 전체 메일 리스트는 mail_no를 switch의 id에 갖고 있거든. ✅ 2024-03-28
		5. [x] Q. agent_no는 고정, 발송기 이름이 변경될때 기존 badge의 이름을 찾아서 변경하려면?. (단 동일한 이름이 여러개일경우를 주의해야 함) ✅ 2024-03-28
		6. [x] badge의 data에 agent_no를 추가하고 agent_no가 일치하면 이름 변경 ✅ 2024-03-28
		7. [ ] 테스트 케이스
			1. [x] 발송기 이름이 변경되면 socket으로 다른 클라이언트에게 알림이 가야 한다. ✅ 2024-03-28
			2. [x] badge의 이름이 변경되어야 한다. ✅ 2024-03-28
				1. [x] 나 자신 ✅ 2024-03-28
				2. [x] 타 클라이언트 ✅ 2024-03-28
				3. [x] 할당/비할당된 이메일의 badge ✅ 2024-03-28