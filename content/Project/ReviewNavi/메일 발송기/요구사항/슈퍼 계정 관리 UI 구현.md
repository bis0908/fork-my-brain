---
Create at: 2024-07-23
tags:
  - Javascript
  - Nodejs
---
---

##### 시나리오
- 슈퍼 계정 관리를 UI에서 할 수 있도록 구현
- UI에서 슈퍼 계정을 추가/삭제할 수 있어야 한다.
- 즉시 발송 토글 기능으로 현재 발송중이어도 즉시 메일이 발송 가능해야 한다.

```sql
-- 테이블 정보
`super_id_list` (
	`no` INT(11) NOT NULL AUTO_INCREMENT,
	`super_id` VARCHAR(50) NOT NULL DEFAULT '' COLLATE 'utf8_general_ci',
	`is_emergency` ENUM('Y','N') NOT NULL DEFAULT 'N' COLLATE 'utf8_general_ci',
	`reg_date` DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
	PRIMARY KEY (`no`) USING BTREE
)
```

##### 구현
- [ ] DB에서 모든 슈퍼 계정 리스트를 불러온다
- [ ] 불러온 리스트로 부트스트랩 modal ui로 구현한다.
	- [ ] 리스트로 구현하고, 왼쪽에 체크박스, 가운데에 계정명, 오른쪽에 삭제 버튼, 즉시발송 토글UI 버튼을 추가한다.
- [ ] 하단에는 신규 슈퍼계정을 추가할 수 있는 입력창과 추가 버튼을 왼쪽에 배치한다.
	- [ ] 엔터키를 눌러도 추가될 수 있도록 구현한다.
	- [ ] 추가 버튼을 누르면 db에 슈퍼 계정이 추가되고 UI 맨 마지막에 추가된다.
- [ ] 삭제 버튼을 누르면 db 테이블에 일치하는 계정이 제거되고 현재 row도 제거된다.
- [ ] 즉시 발송 토글이 on이 되면 is_emergency 필드 값이 Y로 변경된다.
	- [ ] off가 되면 N으로 변경되어야 한다.
	- [ ] 토글이 Y일 경우 발송 스케줄에 가장 최근에 들어오면 즉시 발송될 수 있도록 기존 로직을 변경해야 한다.