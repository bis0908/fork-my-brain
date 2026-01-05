---
title: 레이어별 아키텍처
Create at: 2026-01-05
tags:
  - architecture
  - Nodejs
---

# 레이어별 아키텍처

## 1. 아키텍처 개요
```mermaid
graph TB
    subgraph PL["화면 계층"]
        Views[EJS 템플릿]
        Static[정적 파일]
        Bundle[Webpack 번들]
    end

    subgraph RL["라우터 계층"]
        Auth[인증]
        Crawl[크롤링]
        Mail[메일 발송]
        DB[DB 쿼리]
        Vendor[라이브러리 연동]
    end

    subgraph SL["서비스 계층"]
        AuthS[인증 처리]
        SearchS[블로그 크롤러]
        MailS[메일 발송]
        QueryS[DB 쿼리]
        SocketS[실시간 알림]
        HiworksS[Hiworks 연동]
    end

    subgraph DL["데이터 계층"]
        Pool[커넥션 풀]
        MySQL[(MySQL)]
    end

    subgraph EL["외부 연동 계층"]
        Naver[네이버 API]
        BlogAPI[BlogSecret API]
        SMTP[SMTP 서버]
    end

    PL --> RL
    RL --> SL
    SL --> DL
    SL --> EL
    DL --> MySQL
```

---

## 2. Presentation Layer
### 뷰 구조
```mermaid
graph LR
    subgraph Templates["EJS 템플릿"]
        Login[login.ejs]
        Index[index.ejs]
        Schedule[mail-delivery-schedule.ejs]
        Group[mail-delivery-group-state.ejs]
        ScheduleV2[mail-delivery-schedule-v2.ejs]
    end

    subgraph Bundles["Webpack 번들"]
        LoginB[login.bundle.js]
        IndexB[index.bundle.js]
        MailB[mail_delivery.bundle.js]
    end

    subgraph Assets["정적 자산"]
        CSS[CSS]
        Images[이미지]
        Libs[라이브러리]
    end

    Login --> LoginB
    Index --> IndexB
    Schedule --> MailB
```

### 페이지 접근 권한
| 페이지 | 경로 | 인증 필요 |
|--------|------|----------|
| 로그인 | `/login` | X |
| 대시보드 | `/` | O |
| 메일 스케줄 | `/mail-delivery-schedule` | O |
| 그룹 상태 | `/mail-delivery-group-state` | O |
| 스케줄 V2 | `/mail-delivery-schedule-v2` | O |

---

## 3. Router Layer
### 라우터 상세 구조
```mermaid
graph TB
    subgraph AuthRouter["인증 /auth"]
        A1["로그인"]
        A2["비밀번호 변경"]
    end

    subgraph CrawlRouter["크롤링 /crawling, /api"]
        C1["진행률 조회"]
        C2["진행률 삭제"]
        C3["목록 수집"]
        C4["상세 처리"]
        C5["검색"]
    end

    subgraph MailRouter["메일 발송 /mail"]
        M1["전체 테스트"]
        M2["컨텐츠 테스트 발송"]
        M3["Hiworks 메일 추가"]
        M4["Hiworks 메일 삭제"]
    end

    subgraph DbRouter["DB 쿼리 /db"]
        D1["발신자 목록 조회"]
        D2["발신자 단건 조회"]
        D3["발신자명 변경"]
        D4["수동 추가"]
        D5["발신자 이메일 조회"]
        D6["발신자 메일 추가"]
        D7["블랙리스트 추가"]
        D8["발신자 삭제"]
        D9["검색결과 저장"]
        D10["발송목록 초기화"]
        D11["HTML 템플릿 조회"]
    end
```

### 라우터 - 서비스 의존성
```mermaid
graph LR
    subgraph Routers["라우터"]
        AR[인증]
        CR[크롤링]
        MR[메일]
        DR[DB]
        VR[벤더 연동]
    end

    subgraph Services["서비스"]
        AS[인증 처리]
        DS[블로그 크롤러]
        MS[메일 발송]
        QS[DB 쿼리]
        HS[Hiworks 연동]
    end

    AR --> AS
    CR --> DS
    CR --> QS
    MR --> MS
    MR --> HS
    DR --> QS
    DR --> DS
    DR --> HS
```

---

## 4. Service Layer
### 서비스 책임 분리
```mermaid
graph TB
    subgraph authService["인증 처리"]
        AS1[로그인 검증]
        AS2[비밀번호 변경]
    end

    subgraph doSearchService["블로그 크롤러"]
        DS1[블로그 ID 수집]
        DS2[블록별 목록 수집]
        DS3[포스트 일괄 처리]
        DS4[ID 불일치 검증]
        DS5[키워드 정보 조회]
        DS6[이메일 검증]
    end

    subgraph mailService["메일 발송"]
        MS1[발송 정보 확인]
        MS2[테스트 메일 발송]
        MS3[전송 객체 생성]
        MS4[SMTP 동적 설정]
        MS5[스케줄 처리]
    end

    subgraph queryService["DB 쿼리"]
        QS1[발신자 목록 조회]
        QS2[발신자 단건 조회]
        QS3[발신자 메일 추가]
        QS4[발신자 삭제]
        QS5[검색결과 저장]
        QS6[수신거부 확인]
        QS7[실시간 ID 체크]
        QS8[회원 ID 조회]
    end

    subgraph socketService["실시간 알림"]
        SS1[Socket.IO 초기화]
        SS2[이벤트 핸들러]
    end

    subgraph hiworksService["Hiworks 연동"]
        HS1[Hiworks 메일 추가]
        HS2[Hiworks 메일 삭제]
        HS3[계정명 변경]
    end
```

### 서비스 간 의존성
```mermaid
graph TD
    메일발송[메일 발송] --> DB쿼리[DB 쿼리]
    블로그크롤러[블로그 크롤러] --> DB쿼리
    크롤링라우터[크롤링] --> 블로그크롤러
    크롤링라우터 --> DB쿼리
    DB라우터[DB 쿼리 라우터] --> DB쿼리
    DB라우터 --> 블로그크롤러
    DB라우터 --> Hiworks연동[Hiworks 연동]
```

---

## 5. Data Layer
### 데이터베이스 연결 구조
```mermaid
graph TB
    subgraph Config["DB 설정"]
        Pool[커넥션 풀]
        ExecQuery[쿼리 실행]
    end

    subgraph Tables["주요 테이블"]
        T1[발신자 정보]
        T2[발송 스케줄]
        T3[발신자 그룹]
        T4[메일 서버 상태]
        T5[블랙리스트]
        T6[검색 결과]
    end

    Pool --> T1
    Pool --> T2
    Pool --> T3
    Pool --> T4
    Pool --> T5
    Pool --> T6
    ExecQuery --> Pool
```

### 테이블 관계도
```mermaid
erDiagram
    mail_agent ||--o{ mail_delivery_schedule : "발신자"
    mail_sender_group ||--o{ mail_delivery_schedule : "발송 스케줄러"
    mail_agent ||--o{ search_list : "업체 담당자"

    mail_agent {
        int no PK
        string email
        string password
        string name
    }

    mail_delivery_schedule {
        int no PK
        int sender_group FK
        string send_status
        string reservation_sent
        datetime dispatch_registration_time
    }

    mail_sender_group {
        int no PK
        string title
        text contents
    }

    search_list {
        int no PK
        string id
        string keyword
        string link
        string title
        int sender_id FK
    }

    blacklist {
        int no PK
        string email
        datetime registered_date
    }
```

---

## 6. External Layer
### 외부 시스템 연동
```mermaid
graph TB
    subgraph Internal["내부 서비스"]
        Search[블로그 크롤러]
        Mail[메일 발송]
        Hiworks[Hiworks 연동]
    end

    subgraph NaverAPI["네이버 API"]
        N1[모바일 검색 API]
        N2[블로그 상세 API]
    end

    subgraph BlogSecret["BlogSecret API"]
        BS1[Load Balancer]
        BS2[키워드 정보]
        BS3[병렬 처리]
    end

    subgraph SMTP["SMTP 서버"]
        S1[Gmail SMTP]
        S2[Naver SMTP]
        S3[Hiworks SMTP]
        S4[Daum SMTP]
    end

    Search -->|HTTP| N1
    Search -->|HTTP| N2
    Search -->|HTTP| BS1

    Mail -->|SMTP:587| S1
    Mail -->|SMTP:587| S2
    Mail -->|SMTP:465| S3
    Mail -->|SMTP:465| S4

    Hiworks -->|API| S3
```

### SMTP 동적 설정
| 도메인 | 호스트 | 포트 | 보안 |
|--------|--------|------|------|
| gmail.com | smtp.gmail.com | 587 | TLS |
| naver.com | smtp.naver.com | 587 | STARTTLS |
| daum.net | smtp.daum.net | 465 | TLS |
| 기타 | smtps.hiworks.com | 465 | TLS |

---

## 7. 미들웨어 체인
```mermaid
graph LR
    Request[요청] --> URLEnc[urlencoded]
    URLEnc --> Session[express-session]
    Session --> Static[정적 파일]
    Static --> JSON[JSON 파서]
    JSON --> Auth{인증 체크}
    Auth -->|인증됨| Router[라우터]
    Auth -->|미인증| Login[로그인 리다이렉트]
    Router --> Response[응답]
```

---

## 8. 관련 문서
- [01-system-overview.md](./01-system-overview.md) - 시스템 전체 개요
- [03-crawling-flow.md](./03-crawling-flow.md) - 웹 크롤링 상세 흐름
- [04-email-campaign-flow.md](./04-email-campaign-flow.md) - 이메일 캠페인 상세
- [05-auth-flow.md](./05-auth-flow.md) - 인증 흐름 상세
