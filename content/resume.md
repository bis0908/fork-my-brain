---
title: this.소개
tags:
  - resume
---
---

# 공원석

## Software Engineer

### PROFESSIONAL SUMMARY

9년 산업 현장 운영 경험을 기반으로 안정성과 확장성을 설계하는 풀스택 엔지니어. 시간당 1,200건 처리 이메일 마케팅 플랫폼을 3년간 무중단 운영하며 분산 시스템 설계 역량을 검증했고, 최대 100명 동시 접속 WebRTC SFU 서버 구축으로 실시간 통신 아키텍처 전문성을 확보. 24/7 엔터프라이즈 시스템 구축 경험으로 설계 단계부터 장애 복구와 모니터링을 고려하며, 부하 테스트와 프로파일링을 통한 성능 최적화에 강점을 보유.

### CORE COMPETENCIES

**Languages & Frameworks:** Node.js, Express, Next.js, React  
**Database:** MySQL, Supabase  
**Infrastructure:** AWS EC2/ELB, PM2, CloudType, Docker  
**Real-time:** WebRTC (mediasoup SFU), Socket.IO, Fabric.js  
**Testing & Monitoring:** Jest, Loadero, v8-profiler
**DevOps:** GitHub Actions, Nginx, Cloudflare Tunnel


---

### PROFESSIONAL EXPERIENCE

#### Lead Software Engineer (Independent) | 프리랜서
*2022.12 ~ 현재 (3년)*

---

#### 1. [[네이버 인플루언서 마케팅 플랫폼 구축]] (71,000 라인, 풀스택 100% 단독 개발)

**비즈니스 임팩트:** 1,500만 블로그 중 상위노출 이력 기반 인플루언서 자동 발굴 및 이메일 마케팅 자동화

**핵심 성과:**
- **3년간 발송 누락 사고 0건** 달성 (DB 폴링 기반 분산 처리, FOR UPDATE 락)
- **시간당 1,200건 이메일 발송** 안정 처리, 서버 추가 시 **선형 확장** 가능 (5대 = 6,000건/h)
- **크롤링 속도 3배 향상** (30~60초 → 5~15초, AWS ELB 병렬 처리)
- 네이버 스팸 필터 회피율 99%+ 유지 (딜레이 순차 발송, User Agent 로테이션, 본문 변조)

**기술적 의사결정:**

**[문제 1] 대량 발송 중복 방지**
- 상황: 다중 서버 환경에서 동일 수신자에게 중복 발송 리스크
- 해결: MySQL `FOR UPDATE` 락 기반 분산 작업 할당, 3초 간격 DB 폴링 방식 선택
- 이유: 메시지 큐(RabbitMQ) 대비 인프라 단순화, ACID 트랜잭션 보장
- 결과: 추가 서버 비용 0원, 3년간 중복 발송 0건

**[문제 2] 크롤링 성능 병목**
- 상황: Puppeteer 단일 처리로 키워드당 30~60초 소요
- 해결: AWS ELB 뒤 크롤링 서버 분산, Promise.all 동시성 제한 패턴 
- 결과: 평균 5~15초로 단축, 실시간 진행률 추적 (Socket.IO)

**[문제 3] 서버 장애 자동 복구**
- 상황: 발송 중 서버 다운 시 작업 손실 위험
- 해결: 10초 간격 Heartbeat 메커니즘, MAC 주소 기반 자동 등록
- 결과: 신규 서버 추가 시 설정 파일 수정 불필요, 30초 이내 자동 작업 인계

**AI 통합:**
- Claude/Gemini API 연동, Streaming 응답 처리로 마케팅 콘텐츠 자동 생성
- Quill.js 에디터 실시간 텍스트 삽입, AbortController 기반 생성 중단 기능

**기술 스택:** Node.js, Express, MySQL, Puppeteer, AWS ELB, Nodemailer, Socket.IO, Claude API

---

#### 2. WebRTC 실시간 협업 서비스 (SFU 아키텍처, 최대 100명 지원)

**비즈니스 목표:** 여러 프로토콜(WebRTC/WebSocket/Canvas)이 협업하는 실시간 통신 시스템 학습 및 구축

**핵심 성과:**
- **P2P → SFU 전환으로 확장성 25배 향상** (4명 → 100명 동시 접속)
- **클라이언트 CPU 부하 80% → 15%** 감소 (N² 문제 해결)
- **부하 테스트로 이중 병목 구조 발견** (low spec의 클라우드 컴퓨팅 머신의 네트워크 434Mbps → CPU 50%, 참가자 3배 증가 시 Jitter 1.4배만 증가)
- **3단계 Simulcast 구현** (100k/300k/900k bps 자동 조절)

**아키텍처 설계:**

**[문제 1] P2P N² 확장성 한계**
- 상황: 5명 이상 접속 시 클라이언트 CPU 80% 초과, 연결 불안정
- 해결: mediasoup SFU 아키텍처 도입, Worker 풀 라운드 로빈 할당
- 설계: Manager-Handler 패턴으로 6개 리소스 관리 모듈 분리 (WorkerPool/Router/Transport/Producer/Consumer/ResourceCleaner)
- 결과: E2E Latency 200ms 미만 유지, 최대 100명 지원

**[문제 2] 화면 공유 충돌**
- 상황: 카메라와 화면 공유 Producer가 kind(video)만으로 충돌
- 해결: appData에 `isScreenShare` 플래그 추가, socketId 기반 구분
- 학습: 다중 스트림 시나리오를 설계 단계에서 고려 필요

**[문제 3] ICE 연결 실패 (Cloudflare Tunnel 테스트 환경)**
- 상황: 로컬에서 정상 동작하지만 원격 부하 테스트 시 연결 성공률 0%
- 원인: announcedIp가 127.0.0.1로 하드코딩됨
- 해결: 환경 변수 분리 (LOCAL/STAGING/PROD), TCP fallback 활성화
- 도구: `chrome://webrtc-internals`로 ICE candidate 상태 분석

**모니터링 및 최적화:**
- 커스텀 트래픽 로거 (mediasoup `getStats()` API 기반, 1초 간격 메트릭 수집)
- Loadero 클라우드 부하 테스트 (최대 30명, 1080P@30FPS)
- v8-profiler-next 기반 Node.js CPU 프로파일링, Flame Graph 분석

**기술 스택:** Node.js, Express, mediasoup v3, Socket.IO, Next.js 16, React 19, Fabric.js, shadcn/ui

**문서:** Architecture, Socket Event Spec, Load Test Report, Troubleshooting 기록 (Wiki 6개 문서)

---

#### 3. 밸류앤플러스 사내 생산성 도구 개발 (계약직, 2025.05 ~ 2025.08)

**비즈니스 임팩트:** 비개발 직군의 블로그 마케팅 업무 자동화 (6개 도구 개발)

**주요 구현:**
- 키워드 추출기: GPT API 연동, 블로그 카테고리별 포스트 수집 및 연관 키워드 자동 생성
- 블로그 툴킷: 카테고리/포스트 크롤링, 스마트 블록 랭킹, 플레이스 리뷰 수집
- 포스팅 재사용 검사기: 포스팅 원고를 관리하는 블로그에서 포스팅 재사용 기록을 확인하기 위한 웹 서비스
	- Next.js 기반 웹 앱, 관리자 대시보드, 사용자 통계 분석
- 이미지 다운로더 & 변형 도구 - 블로그 이미지 자동 다운로드 및 이미지 합성 변형

**기술 스택:** Python, Node.js, Supabase, Next.js

---

#### 4. 기타 프로젝트

**대학 입시 컨설팅 사이트** | [라이브 페이지](https://seedconsulting.co.kr/)
- Node.js, Express, MySQL, Vanilla JS/jQuery 기반 풀스택 개발

**SFU WebRTC 강의 시스템 협업 참여**
- 결제 시스템 구현 (Node.js, Socket.IO, mediasoup, MySQL)

**수제 콜라 홍보 랜딩 페이지** | [라이브 페이지](https://www.tantscola.com/)
- 브랜드 스토리텔링 중심 반응형 웹 디자인

---

### Software Engineer | 비카누스
*2022.01 ~ 2022.11 (11개월)*

**컨테이너화된 풀스택 환경 경험 및 실시간 데이터 파이프라인 구축**

**Fanuc 로봇 3D 스캐닝 시스템 유지보수**
- 기술 스택: React, Spring Boot, Redis, Docker, MySQL, Nginx
- 레거시 코드 분석 및 버그 수정, Docker 기반 컨테이너 환경에서 협업 경험
- React 컴포넌트 리팩토링을 통한 코드 재사용성 개선

**DAQ 데이터 수집 시스템 유지보수**
- 기술 스택: React, Spring Boot, Message Broker, InfluxDB, Nginx
- 학습: 시계열 데이터 특성 이해, InfluxDB Flux 쿼리 언어 습득
- 기술 선택 배경: MySQL 대비 시계열 특화 DB(Downsampling, Retention Policy)의 필요성 이해
- 기여: Message Broker 기반 실시간 데이터 스트리밍 파이프라인 구축 참여, Grafana 대시보드 개발

**협업 경험:**
- Git 브랜치 전략, 코드 리뷰 프로세스 경험
- 4인 풀스택 팀에서 프론트엔드 담당

---

## TECHNICAL PROJECTS

### 젠더 리빌 사이트 (완전 무료) | [라이브 페이지](https://gender-reveal-theta.vercel.app/)

**목적:** 미국 트렌드인 Gender Reveal Party를 온라인으로 구현, SNS 공유 가능

**개발 속도:** Cursor AI 활용으로 2일 내 프로토타이핑 및 Vercel 배포

**주요 기능:**
- 4종 젠더 리빌 애니메이션, 단태아/다태아 구분
- 커스텀 카운트다운 타이머

**기술 스택:** Next.js, shadcn/ui, TypeScript, Vercel

---

## PRE-DEVELOPMENT CAREER HIGHLIGHTS

**차별화 포인트:** 9년간 24/7 산업 시스템 운영 경험으로 안정성 설계 관점 보유

### 엔클로니 | 시스템 운영팀 대리 (2016.06 ~ 2020.12)

**제약 자동화 장비 전문 기술 엔지니어 (2D/3D 비전 검사 시스템)**

**핵심 역할: 글로벌 제약사 대상 검사 장비 납품 및 현장 기술 지원**

**시스템 기술 전문성**
- 장비: Planet EV 시리즈 (정제/캡슐 외관 검사 장비)
- 검사 기술: 2D 카메라(정면/측면), 3D 카메라(깊이 검출), IR 카메라(캡슐 내부 투과)
- 성능: 시간당 최대 42만 정 검사 능력, 사각지대 없는 360도 검사
- 검출 항목: 크랙, 파손, 오염, 색상 불량, 각인 결함, 캡슐 충진량 측정

**세계 최초 기술 지원 경험 (2020년)**
- 프로젝트: Planet DLPI (외관 검사 + 레이저 인쇄 겸용 장비)
- 혁신: 불량 선별과 식별마크 레이저 인쇄를 단일 공정으로 통합
- 의의: 기존 2단계 공정을 1단계로 단축하여 생산성 개선
- 기술 난이도: 정제 양면 동시 인쇄, 고속 검사 중 실시간 레이저 각인

**글로벌 제약사 납품 실적**
- 해외: Johnson & Johnson, 화이자(Pfizer)
- 국내: 78개 제약사 (대웅제약, 유한양행, 한미약품 등 주요 제약사 포함)
- 독일/일본 법인을 통한 현지화 기술 지원

**해외 프로젝트 및 전시회**
- 출장 국가: 인도, 말레이시아, 일본, 독일, 중국
- 역할: 현장 설치, 커미셔닝, 검수, 장비 데모
- 전시회: 한국, 미국, 독일, 일본 제약 전시회 참가
- 성과: 현장 데모를 통한 수주 전환, 글로벌 제약사 네트워크 구축

**현장 기술 지원 및 문제 해결**
- 24/7 온콜 체제: 제약사 생산 라인 긴급 장애 대응
- 설치 프로세스: 장비 운송 → 현장 설치 → 비전 시스템 캘리브레이션 → 생산 라인 연동 → 검수
- 트러블슈팅: 카메라 초점 조정, 조명 최적화, 컨베이어 속도 동기화, 불량 판정 임계값 튜닝
- 고객 교육: 오퍼레이터 교육, 유지보수 매뉴얼 작성, 소모품 교체 절차 전수

**제약 산업 품질 규정 이해**
- GMP(Good Manufacturing Practice) 준수: 의약품 제조 및 품질 관리 기준 이해
- Validation 프로세스: IQ(Installation Qualification), OQ(Operational Qualification), PQ(Performance Qualification) 경험

**핵심 경험**
- 비전 검사 시스템 튜닝: 조명, 카메라 각도, 임계값 조정으로 검출률 최적화
- 다국적 프로젝트 관리: 시차, 언어, 문화 차이 극복한 원격 기술 지원
- 고객 중심 커뮤니케이션: 제약사 생산팀과 기술 이슈 협의, 맞춤형 솔루션 제안
- 긴급 장애 대응: 생산 라인 가동 중단 최소화를 위한 신속한 현장 조치

**기술 스택:** 2D/3D 머신 비전, IR 이미징, 레이저 마킹, 컨베이어 동기화, GMP

### ㈜오엔테크놀러지 | 필드 엔지니어 대리 (2013.07 ~ 2015.04)

**발전소 자동화 및 예지보전 시스템 구축 전문**

**한국남부발전 삼척그린파워 CMMS 구축 프로젝트**
- 프로젝트: Emerson 자산 성능 관리 솔루션 기반 설비 유지보수 시스템 구축
- 역할: 필드 엔지니어로 DCS(분산제어시스템) 및 Ovation™ 제어 시스템 연동 담당
- 범위: 보일러, 터빈 등 발전소 핵심 설비 자산 관리 데이터베이스 구축
- 기술: 예측 기술, CMMS 데이터 통합, 문서화된 작업 프로세스 관리
- 학습: 24/7 운영되는 엔터프라이즈급 시스템의 데이터 정합성 및 가용성 요구사항 이해

**한국수력원자력 발전소 진동 측정 시스템 구축**
- 시스템: Emerson AMS (Asset Management System) 진동 모니터링 솔루션
- 장비: AMS 무선 진동 모니터, 회전 장비(펌프, 터빈, 모터) 예지보전용 센서
- 역할: 현장 설치, 설비 커미셔닝, AMS Machine Works 소프트웨어 교육
- 기술 지원: PeakVue™ Plus 기술 기반 베어링 결함, 기어 마모, 윤활 부족 등 자동 진단
- 성과: 회전 장비 고장 예측으로 비계획 정지 시간 최소화, 발전소 안정성 기여

**핵심 경험:**
- 고신뢰성 요구 환경: 원자력/화력 발전소의 무중단 운영 요구사항 경험
- 긴급 장애 대응: 발전소 돌발 상황 시 신속한 현장 기술 지원 (24시간 온콜 체제)
- 엔지니어링 문서화: 설치 매뉴얼, 운영 절차서, 트러블슈팅 가이드 작성
- 고객 커뮤니케이션: 발전소 운영팀과 기술 이슈 협의 및 교육 진행

**기술 스택:** Emerson Ovation DCS, AMS Machine Works, CMMS, 진동 분석, 예지보전

### SK 커뮤니케이션즈 자회사 | 품질관리팀 사원 (2011.07 ~ 2013.06)
- 싸이월드 앱 품질 테스트, 네이트 메인 콘텐츠 사용자 통계 관리
- **학습:** QA 관점에서의 사용자 경험 검증, 데이터 분석 기초

---

## EDUCATION

**빅데이터 분석 전문가 과정** | 2020.12 ~ 2021.04
- Python, R 기반 대용량 데이터 분석, 시각화

---
