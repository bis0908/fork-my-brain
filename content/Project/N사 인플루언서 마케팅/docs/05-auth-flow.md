---
Create at: 2026-01-05
title: 인증 흐름 상세
tags:
  - architecture
  - Nodejs
---

# 인증 흐름 상세

## 1. 인증 시스템 개요
express-session 기반의 세션 인증 시스템

### 핵심 구성요소
- **메인 앱**: 세션 미들웨어 및 인증 체크
- **인증 라우터**: 인증 API 엔드포인트
- **인증 서비스**: 인증 비즈니스 로직

---

## 2. 세션 구성
```mermaid
graph TB
    subgraph SessionConfig["express-session 설정"]
        A[secret: crypto-random-string<br/>48자 base64]
        B[resave: false]
        C[saveUninitialized: true]
    end
```

### 세션 저장소
- 기본 메모리 저장소 사용
- 서버 재시작 시 세션 초기화

---

## 3. 인증 흐름
### 3.1 로그인 흐름
```mermaid
sequenceDiagram
    participant U as 사용자
    participant B as 브라우저
    participant A as 메인 앱
    participant R as 인증 라우터
    participant S as 인증 서비스

    U->>B: 비밀번호 입력
    B->>A: POST /auth/login
    A->>R: 라우터 처리
    R->>S: serverLogin(password)

    S->>S: 비밀번호 검증
    S-->>R: true/false

    alt 인증 성공
        R->>R: req.session.loggedin = true
        R-->>B: {success: true}
        B->>A: GET /
        A->>A: checkAuthenticated
        A-->>B: index.ejs 렌더링
    else 인증 실패
        R-->>B: {success: false}
        B->>U: 로그인 실패 표시
    end
```

### 3.2 인증 체크 미들웨어
```mermaid
flowchart TD
    A[요청 수신] --> B{req.session.loggedin?}
    B -->|true| C[next 계속 진행]
    B -->|false| D[ /login 리다이렉트]
```

---

## 4. API 엔드포인트
### 4.1 로그인 API
```js
POST /auth/login
```

**요청:**

```json
{
  "password": "string"
}
```

**응답:**

```json
{
  "success": true | false
}
```

### 4.2 비밀번호 변경 API
```js
POST /auth/changePassword
```

**요청:**

```json
{
  "currentPw": "string",
  "newPw": "string"
}
```

### 4.3 로그아웃 API
```js
POST /logout
```

```mermaid
sequenceDiagram
    participant U as 사용자
    participant A as 메인 앱

    U->>A: POST /logout
    A->>A: session.destroy()
    A-->>U: 리다이렉트 "/"
```

---

## 5. 페이지 보호
### 5.1 보호 대상 페이지
```mermaid
graph TB
    subgraph Protected["인증 필요"]
        A["/dashboard"]
        B["/mail"]
    end

    subgraph Public["공개 접근"]
        E["/login"]
        F["/auth/*"]
        G[정적 파일]
    end
```

### 5.2 접근 제어 흐름
```mermaid
flowchart TD
    A[사용자 요청] --> B{URL 확인}

    B -->|/login| C[로그인 페이지 표시]
    B -->|/auth/*| D[인증 API 처리]
    B -->|정적 파일| E[파일 제공]
    B -->|보호 페이지| F{세션 확인}

    F -->|인증됨| G[페이지 렌더링]
    F -->|미인증| H[ /login 리다이렉트]
```

---

## 6. 로그인 페이지
### 6.1 페이지 렌더링
```mermaid
sequenceDiagram
    participant U as 사용자
    participant A as 메인 앱
    participant V as 로그인 템플릿

    U->>A: GET /login
    A->>A: 비밀번호 길이 설정 조회
    Note over A: LOGIN_PASSWORD_LENGTH: 6
    A->>V: render("login", {passwordLength})
    V-->>U: 로그인 폼 표시
```

### 6.2 환경 설정
| 변수 | 설명 | 기본값 |
|------|------|--------|
| LOGIN_PASSWORD_LENGTH | 비밀번호 입력 필드 길이 | 6 |

---

## 7. 세션 관리
### 7.1 세션 생명주기
```mermaid
stateDiagram-v2
    [*] --> 미인증: 첫 접속
    미인증 --> 인증됨: 로그인 성공
    인증됨 --> 미인증: 로그아웃
    인증됨 --> 미인증: 세션 만료
    인증됨 --> [*]: 브라우저 종료
```

### 7.2 세션 데이터
```mermaid
graph TB
    subgraph SessionData["req.session"]
        A[loggedin: boolean]
        B[cookie: SessionCookie]
    end
```

---

## 8. 보안 고려사항
### 8.1 현재 구현
```mermaid
graph TB
    subgraph Implemented["구현됨"]
        A[랜덤 세션 시크릿]
        B[세션 기반 인증]
        C[비밀번호 기반 로그인]
    end

    subgraph NotImplemented["미구현"]
        D[HTTPS 쿠키]
        E[CSRF 토큰]
        F[비밀번호 해싱]
        G[로그인 시도 제한]
    end
```

### 8.2 세션 시크릿 생성
```mermaid
flowchart LR
    A[crypto-random-string] --> B[48자]
    B --> C[base64 인코딩]
    C --> D[세션 시크릿]
```

---

## 9. 에러 처리
```mermaid
flowchart TD
    A[인증 요청] --> B{처리 중 에러?}

    B -->|No| C[정상 응답]
    B -->|Yes| D[logger.error 로깅]
    D --> E["500 Internal Server Error"]
```

---

## 10. 코드 구조
### 10.1 인증 라우터
```mermaid
graph TB
    subgraph AuthRouter["인증 라우터"]
        A["POST /login"] --> A1[로그인 검증 호출]
        A1 --> A2{성공?}
        A2 -->|Yes| A3[session.loggedin = true]
        A2 -->|No| A4[success: false 반환]

        B["POST /changePassword"] --> B1[비밀번호 변경 호출]
        B1 --> B2[결과 반환]
    end
```

### 10.2 인증 서비스
```mermaid
graph TB
    subgraph AuthService["인증 서비스"]
        A[로그인 검증]
        B[비밀번호 변경]
    end
```

---

## 11. 통합 흐름도
```mermaid
flowchart TD
    subgraph Entry["진입점"]
        A[사용자 접속]
    end

    subgraph Auth["인증 단계"]
        B{로그인 상태?}
        C[로그인 페이지]
        D[비밀번호 입력]
        E{검증 성공?}
        F[세션 생성]
    end

    subgraph App["애플리케이션"]
        G[대시보드]
        H[메일 스케줄]
        I[그룹 상태]
    end

    subgraph Exit["종료"]
        J[로그아웃]
        K[세션 파기]
    end

    A --> B
    B -->|No| C
    B -->|Yes| G
    C --> D
    D --> E
    E -->|Yes| F
    E -->|No| C
    F --> G
    G --> H
    G --> I
    G --> J
    H --> J
    I --> J
    J --> K
    K --> C
```

---

## 12. 관련 문서
- [01-system-overview.md](./01-system-overview.md) - 시스템 전체 개요
- [02-layer-architecture.md](./02-layer-architecture.md) - 레이어별 상세 구조
- [03-crawling-flow.md](./03-crawling-flow.md) - 웹 크롤링 상세 흐름
- [04-email-campaign-flow.md](./04-email-campaign-flow.md) - 이메일 캠페인 상세
