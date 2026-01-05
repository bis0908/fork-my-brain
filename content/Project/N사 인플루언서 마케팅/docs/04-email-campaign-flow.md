---
Create at: 2026-01-05
title: 이메일 캠페인 상세 흐름
tags:
  - architecture
  - Nodejs
---

# 이메일 캠페인 상세 흐름

## 1. 이메일 시스템 개요
Nodemailer 기반의 다중 SMTP 지원 이메일 발송 시스템

### 핵심 구성요소
- **메일 라우터**: 메일 API 엔드포인트
- **메일 발송**: 메일 발송 비즈니스 로직
- **Hiworks 연동**: 하이웍스 메일 계정 관리
- **DB 쿼리**: 발신자/수신자 DB 관리

---

## 2. 전체 이메일 발송 흐름
```mermaid
sequenceDiagram
    participant U as 사용자
    participant R as 메일 라우터
    participant M as 메일 발송
    participant Q as DB 쿼리
    participant DB as MySQL
    participant SMTP as SMTP 서버

    rect rgb(240, 255, 240)
        Note over U,DB: 1. 발송 준비
        U->>R: 캠페인 설정
        R->>Q: 발신자 정보 조회
        Q->>DB: mail_agent 조회
        DB-->>Q: 발신자 정보
        Q-->>R: 이메일/비밀번호
    end

    rect rgb(255, 248, 240)
        Note over R,SMTP: 2. 메일 발송
        R->>M: sendTestMailWithContent()
        M->>M: dynamicSMTP() - SMTP 설정
        M->>M: makeTransport() - 전송 객체 생성
        M->>SMTP: 메일 전송
        SMTP-->>M: 발송 결과
    end

    rect rgb(240, 240, 255)
        Note over M,DB: 3. 상태 업데이트
        M->>DB: 발송 상태 저장
        M-->>R: 결과 반환
        R-->>U: 발송 완료
    end
```

---

## 3. 발신자 관리
### 3.1 발신자 구조
```mermaid
erDiagram
    mail_agent {
        int no PK
        string email "발신 이메일"
        string password "앱 비밀번호"
        string name "발신자 이름"
    }

    mail_sender_group {
        int no PK
        string title "그룹 제목"
        text contents "메일 본문"
    }

    mail_agent ||--o{ mail_delivery_schedule : "발송"
```

### 3.2 발신자 API
| 엔드포인트 | 설명 |
|-----------|------|
| POST /db/getSender | 전체 발신자 목록 조회 |
| POST /db/getOneSender | 특정 발신자 조회 |
| POST /db/addSenderMail | 새 발신자 추가 |
| POST /db/deleteSender | 발신자 삭제 |
| POST /db/changeSenderName | 발신자 이름 변경 |

---

## 4. SMTP 동적 설정
### 4.1 SMTP 선택 로직
```mermaid
flowchart TD
    A[발신 이메일] --> B{도메인 확인}

    B -->|gmail.com| C[Gmail SMTP]
    B -->|naver.com| D[Naver SMTP]
    B -->|daum.net| E[Daum SMTP]
    B -->|기타| F[Hiworks SMTP]

    C --> G["smtp.gmail.com:587<br/>TLS"]
    D --> H["smtp.naver.com:587<br/>STARTTLS"]
    E --> I["smtp.daum.net:465<br/>TLS"]
    F --> J["smtps.hiworks.com:465<br/>TLS"]
```

### 4.2 SMTP 설정 상세
```mermaid
graph TB
    subgraph Gmail["Gmail"]
        G1[호스트: smtp.gmail.com]
        G2[포트: 587]
        G3[보안: TLS]
        G4[인증: 앱 비밀번호]
    end

    subgraph Naver["Naver"]
        N1[호스트: smtp.naver.com]
        N2[포트: 587]
        N3[보안: STARTTLS]
        N4[TLS 버전: 1.2+]
    end

    subgraph Hiworks["Hiworks"]
        H1[호스트: smtps.hiworks.com]
        H2[포트: 465]
        H3[보안: TLS]
        H4[계정 관리 API 연동]
    end
```

---

## 5. 메일 발송 상세
### 5.1 테스트 발송 API
```js
POST /mail/sendTestWithContent
```

**요청 파라미터:**

| 필드 | 타입 | 필수 | 설명 |
|------|------|------|------|
| senderId | Number | O | 발신자 번호 |
| testId | String | O | 테스트 수신 이메일 |
| senderName | String | O | 발신자 표시 이름 |
| subject | String | O | 메일 제목 |
| body | String | O | 메일 본문 (HTML) |
| senderGroup | Number | - | 발신자 그룹 |

### 5.2 발송 프로세스
```mermaid
flowchart TD
    A[발송 요청] --> B{필수 파라미터 확인}
    B -->|누락| C[400 에러 반환]
    B -->|OK| D[sendTestMailWithContent]

    D --> E[발신자 정보 조회]
    E --> F[dynamicSMTP 설정]
    F --> G[Transport 생성]
    G --> H[메일 옵션 구성]

    H --> I[제목 랜덤 문자 추가]
    I --> J[본문 랜덤 문자 추가]
    J --> K[푸터 추가]
    K --> L[sendMail 실행]

    L --> M{발송 성공?}
    M -->|Yes| N[성공 응답]
    M -->|No| O[에러 로깅]
    O --> P[500 에러 반환]
```

---

## 6. 메일 본문 처리
### 6.1 본문 가공 기법
> ⚠️ **대외비** - 상세 구현 내용 비공개

```mermaid
graph TD
    subgraph Processing["본문 가공"]
        A1[텍스트 전처리]
        A2[링크 처리]
        A3[푸터 삽입]
    end
```

### 6.2 푸터 구조
```mermaid
graph TB
    subgraph Footer["메일 푸터"]
        A[발송 시간 안내]
        B[수신거부 링크]
        C[회사 정보]
        D[연락처/주소]
    end
```

---

## 7. 스케줄 발송
### 7.1 발송 상태
```mermaid
stateDiagram-v2
    [*] --> immediately: 즉시 발송 요청
    [*] --> scheduled: 예약 발송 요청

    immediately --> processing: 처리 시작
    scheduled --> processing: 예약 시간 도달

    processing --> sent: 발송 성공
    processing --> failed: 발송 실패

    sent --> [*]
    failed --> retry: 재시도
    retry --> processing
```

### 7.2 스케줄 쿼리
```mermaid
flowchart TD
    subgraph Scheduled["예약 발송 조회"]
        A1["send_status = 'scheduled'"]
        A2["reservation_sent = 'Y'"]
        A3["dispatch_registration_time <= NOW()"]
    end

    subgraph Immediately["즉시 발송 조회"]
        B1["send_status = 'immediately'"]
        B2["reservation_sent = 'N'"]
    end

    A1 & A2 & A3 --> C[예약 발송 대상]
    B1 & B2 --> D[즉시 발송 대상]
```

---

## 8. Hiworks 연동
### 8.1 Hiworks API
```mermaid
sequenceDiagram
    participant R as 메일 라우터
    participant H as Hiworks 연동
    participant API as Hiworks API

    R->>H: addNewHiworksMail(email, password)
    H->>API: 계정 생성 요청
    API-->>H: 생성 결과
    H-->>R: {isSuccess, message}

    R->>H: deleteEmailFromHiworks(email)
    H->>API: 계정 삭제 요청
    API-->>H: 삭제 결과
    H-->>R: isSuccess
```

### 8.2 Hiworks API 엔드포인트
| 엔드포인트 | 설명 |
|-----------|------|
| POST /mail/addNewHiworksMail | 새 Hiworks 메일 계정 추가 |
| POST /mail/deleteEmailFromHiworks | Hiworks 메일 계정 삭제 |

---

## 9. 수신자 관리
### 9.1 수신자 필터링
```mermaid
flowchart TD
    A[수신자 목록] --> B{블랙리스트 체크}
    B -->|블랙리스트| C[제외]
    B -->|정상| D{이미 발송됨?}
    D -->|Yes| E[제외]
    D -->|No| F{수신거부?}
    F -->|Yes| G[제외]
    F -->|No| H[발송 대상]
```

### 9.2 관련 API
| 엔드포인트 | 설명 |
|-----------|------|
| POST /db/addBlackList | 블랙리스트 추가 |
| POST /db/addManual | 수동 수신자 추가 (중복/블랙리스트 체크) |
| POST /db/InsertSearchlist | 검색 결과를 발송 목록에 추가 |
| POST /db/sendListClear | 발송 목록 초기화 |

---

## 10. 실시간 상태 업데이트
### 10.1 Socket.IO 이벤트
```mermaid
graph TB
    subgraph Events["Socket.IO 이벤트"]
        A[assigned] --> A1[발신자 할당 알림]
        B[unassigned] --> B1[발신자 해제 알림]
        C[newSenderEmail] --> C1[새 발신자 추가 알림]
        D[deleteSender] --> D1[발신자 삭제 알림]
        E[renameAgent] --> E1[발신자 이름 변경 알림]
        F[unassignAll] --> F1[전체 해제 알림]
    end
```

### 10.2 이벤트 흐름
```mermaid
sequenceDiagram
    participant C1 as 클라이언트1
    participant S as 실시간 알림 서버
    participant C2 as 클라이언트2

    C1->>S: assigned (발신자 할당)
    S->>C2: getEmailStateChanged (상태 변경)

    C1->>S: newSenderEmail (새 발신자)
    S->>C2: newSenderEmail (브로드캐스트)
```

---

## 11. 에러 처리
```mermaid
flowchart TD
    A[메일 발송] --> B{에러 발생?}
    B -->|No| C[성공 응답]
    B -->|Yes| D{에러 유형}

    D -->|인증 실패| E[401 - 비밀번호 확인]
    D -->|연결 실패| F[503 - SMTP 서버 확인]
    D -->|기타| G[500 - 내부 오류]

    E --> H[로깅]
    F --> H
    G --> H
    H --> I[에러 응답]
```

---

## 12. 관련 문서
- [01-system-overview.md](./01-system-overview.md) - 시스템 전체 개요
- [02-layer-architecture.md](./02-layer-architecture.md) - 레이어별 상세 구조
- [03-crawling-flow.md](./03-crawling-flow.md) - 웹 크롤링 상세 흐름
- [05-auth-flow.md](./05-auth-flow.md) - 인증 흐름 상세
