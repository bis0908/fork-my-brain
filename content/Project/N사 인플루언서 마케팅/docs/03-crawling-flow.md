---
Create at: 2026-01-05
title: 웹 크롤링 상세 흐름
tags:
  - architecture
  - Nodejs
---

# 웹 크롤링 상세 흐름
## 1. 크롤링 시스템 개요
네이버 블로그 검색 결과에서 블로거 정보를 수집하고 이메일을 검증하는 시스템

### 핵심 구성요소
- **크롤링 라우터**: API 엔드포인트 정의
- **블로그 크롤러**: 크롤링 비즈니스 로직
- **이메일 검증기**: 이메일 유효성 검증 유틸리티

---

## 2. 전체 크롤링 흐름
```mermaid
sequenceDiagram
    participant C as 클라이언트
    participant R as 크롤링 라우터
    participant S as 블로그 크롤러
    participant N as 네이버 API
    participant B as BlogSecret API
    participant V as 이메일 검증기
    participant DB as MySQL

    rect rgb(240, 248, 255)
        Note over C,R: Phase 1: 목록 수집
        C->>R: POST /collectList
        R->>S: collectListWithBlock()
        S->>N: 네이버 검색 요청
        N-->>S: HTML 응답
        S->>S: HTML 파싱 (Cheerio)
        S-->>R: 블로그 목록
        R-->>C: items[] 반환
    end

    rect rgb(255, 248, 240)
        Note over C,DB: Phase 2: 상세 처리
        C->>R: POST /processPosts
        R->>S: processPostsBatch()
        S->>B: 병렬 상세 수집
        B-->>S: 블로거 상세 정보
        S->>V: 이메일 검증
        V-->>S: 유효 이메일
        S->>DB: 회원 정보 조회
        S-->>R: 처리된 결과
        R-->>C: 최종 결과
    end
```

---

## 3. 목록 수집 단계 (collectList)
### 3.1 API 엔드포인트
```js
POST /crawling/collectList
```

**요청 파라미터:**

| 필드 | 타입 | 설명 |
|------|------|------|
| tasks | Array | 키워드/블록/페이지 정보 |
| score1 | Number | 점수 기준 1 |
| score2 | Number | 점수 기준 2 |
| sessionId | String | 진행률 추적용 세션 ID |

### 3.2 처리 흐름
```mermaid
flowchart TD
    A[collectList 요청] --> B[키워드 정보 미리 로드]
    B --> C{병렬 처리}

    C --> D1[키워드1 + 블록1]
    C --> D2[키워드1 + 블록2]
    C --> D3[키워드2 + 블록1]

    D1 --> E[collectListWithBlock]
    D2 --> E
    D3 --> E

    E --> F[네이버 검색 호출]
    F --> G[HTML 파싱]
    G --> H[블로그 정보 추출]
    H --> I[결과 병합]
    I --> J[응답 반환]

    subgraph Progress["진행률 추적"]
        P1[progressMap]
        P2[현재/전체 페이지]
        P3[phase, message]
    end

    E -.-> P1
```

### 3.3 동시성 제어
```mermaid
graph TD
    subgraph Concurrency["동시성 제한 패턴"]
        A[태스크 큐] --> B{실행 중 < 제한?}
        B -->|Yes| C[태스크 실행]
        B -->|No| D[Promise.race 대기]
        D --> B
        C --> E[완료 시 큐에서 제거]
        E --> B
    end
```

**설정값:**

- `MAX_CONCURRENT_REQUESTS`: 10 (동시 요청 제한)
- `MAX_PAGES_PER_BLOCK`: 5 (블록당 최대 페이지)

---

## 4. 상세 처리 단계 (processPosts)
### 4.1 API 엔드포인트
```js
POST /crawling/processPosts
```

**요청 파라미터:**

| 필드 | 타입 | 설명 |
|------|------|------|
| items | Array | 수집된 블로그 목록 |
| sessionId | String | 진행률 추적용 세션 ID |

### 4.2 처리 흐름
```mermaid
flowchart TD
    A[processPosts 요청] --> B[processPostsBatch]
    B --> C[BlogSecret API 병렬 호출]
    C --> D[블로거 상세 정보 수집]
    D --> E[이메일 검증]
    E --> F[회원 정보 조회]
    F --> G{리뷰나비 회원?}
    G -->|Yes| H[회원 표시 추가]
    G -->|No| I[일반 사용자]
    H --> J[결과 반환]
    I --> J
```

### 4.3 회원 체크 로직
```mermaid
flowchart LR
    A[블로그 ID 목록] --> B[getMemberId 호출]
    B --> C[DB 조회]
    C --> D{회원 존재?}
    D -->|Yes| E["🦋 (닉네임) 추가"]
    D -->|No| F[그대로 유지]
```

---

## 5. 검색 API (search)
### 5.1 API 엔드포인트
```js
POST /crawling/search
```

### 5.2 처리 흐름
```mermaid
sequenceDiagram
    participant C as 클라이언트
    participant R as 크롤링 라우터
    participant S as 블로그 크롤러
    participant N as 네이버
    participant V as 이메일 검증기
    participant DB as MySQL

    C->>R: POST /search (keyword, score1, score2)
    R->>S: crawlingId()

    Note over S,N: 네이버 모바일 검색
    S->>N: GET m.search.naver.com
    N-->>S: HTML 응답

    S->>S: HTML 파싱 (Cheerio)
    S->>S: 블로그 ID 추출
    S->>S: 점수 필터링

    S->>V: validateBlogEmails()
    V-->>S: 유효 이메일 목록

    S->>DB: 기존 발송 체크
    S->>DB: 블랙리스트 체크

    S-->>R: 필터링된 결과
    R-->>C: 최종 결과
```

---

## 6. 네이버 크롤링 상세
### 6.1 검색 URL 구성
```mermaid
graph LR
    A[키워드] --> B[URL 인코딩]
    B --> C["m.search.naver.com"]
    C --> D["?ssc=tab.m_blog.all"]
    D --> E["&sm=mtb_jum"]
    E --> F["&query={keyword}"]
```

### 6.2 HTML 파싱 셀렉터
| 셀렉터 | 추출 대상 |
|--------|----------|
| `.user_info a` | 사용자 정보 |
| `.title_area a` | 포스팅 제목/URL |
| `.dsc_area a` | 설명 영역 |
| `data-cb-target` | 고유 ID (gdid) |

### 6.3 User-Agent 로테이션
```mermaid
graph TD
    A[요청 시작] --> B[userAgents 배열]
    B --> C[랜덤 선택]
    C --> D[요청 헤더에 설정]
    D --> E[HTTP 요청]

    subgraph Agents["User-Agent 풀 (30+)"]
        UA1[Chrome Windows]
        UA2[Chrome Mac]
        UA3[Firefox]
        UA4[Safari]
        UA5[Mobile Chrome]
    end
```

---

## 7. 진행률 추적
### 7.1 진행률 API
```js
GET /crawling/progress/:sessionId
DELETE /crawling/progress/:sessionId
```

### 7.2 진행률 데이터 구조
```mermaid
graph TB
    subgraph ProgressData["progressMap.get(sessionId)"]
        A[current: 현재 완료 수]
        B[total: 전체 수]
        C[phase: 단계]
        D[message: 메시지]
        E[keyword: 현재 키워드]
        F[block: 현재 블록]
        G[updatedAt: 갱신 시간]
    end
```

### 7.3 Phase 상태
| Phase | 설명 |
|-------|------|
| waiting | 대기 중 |
| list-collecting | 목록 수집 중 |
| list-complete | 목록 수집 완료 |
| processing | 상세 처리 중 |
| complete | 전체 완료 |

---

## 8. BlogSecret API 연동
### 8.1 API 구조
```mermaid
graph TB
    subgraph Internal["내부 시스템"]
        Service[블로그 크롤러]
    end

    subgraph External["BlogSecret API"]
        LB[Load Balancer<br/>AWS ELB]
        API1[API 서버 1]
        API2[API 서버 2]
        API3[API 서버 N]
    end

    Service -->|HTTP| LB
    LB --> API1
    LB --> API2
    LB --> API3
```

### 8.2 환경 설정
```js
BLOG_SECRET_API_URL=http://BlogSecretAPI-LoadBalancer-*.elb.amazonaws.com
USE_PARALLEL_API=true
```

---

## 9. 에러 처리
```mermaid
flowchart TD
    A[요청] --> B{성공?}
    B -->|Yes| C[결과 반환]
    B -->|No| D{재시도 가능?}
    D -->|Yes| E[fetchWithRetry]
    D -->|No| F[에러 로깅]
    E --> B
    F --> G[에러 응답]
```

---

## 10. 관련 문서
- [01-system-overview.md](./01-system-overview.md) - 시스템 전체 개요
- [02-layer-architecture.md](./02-layer-architecture.md) - 레이어별 상세 구조
- [04-email-campaign-flow.md](./04-email-campaign-flow.md) - 이메일 캠페인 상세
- [05-auth-flow.md](./05-auth-flow.md) - 인증 흐름 상세
