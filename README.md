<div align="center">

![header](https://capsule-render.vercel.app/api?type=waving&color=auto&height=200&section=header&text=SkyWith&fontSize=50&animation=fadeIn&fontAlignY=38&desc=The%20Software%20Engineer&descAlignY=51&descAlign=62)

</div>

<div align="center">

**LLM 에이전트 · 백엔드를 파고드는 풀스택 개발자 — 인덕대학교 컴퓨터소프트웨어 전공**
*"돈·인증 같은 결정은 결정론으로, 판단은 LLM으로" — 신뢰할 수 있는 자동화를 설계합니다.*

<br>

![GitHub Stats](https://github-readme-stats.vercel.app/api?username=SkyWith628&show_icons=true&hide_border=true&count_private=true&theme=tokyonight)

</div>

---

## 🔥 대표 프로젝트

| 프로젝트 | 분야 | 한 줄 | 코드 / 데모 |
| --- | --- | --- | --- |
| 🐻 **직구곰** | AI · 백엔드 | 구매대행 자동화 — 결정론 파이프라인 + 멀티에이전트 | [Code](https://github.com/SkyWith628/jikgugom) |
| 🧙 **CodeSage** | AI · 백엔드 | GitHub PR 자동 코드리뷰 에이전트 (비동기 큐) | [Code](https://github.com/SkyWith628/codesage) |
| 💎 **CHECKMATE** | 풀스택 웹 | 주얼리 이커머스 (Next.js + Supabase RLS) | [Code](https://github.com/SkyWith628/checkmate) · [Live](https://checkmate-virid-kappa.vercel.app) |
| ♻️ **OBLIGE** | 풀스택 웹 | 공병 반납 ESG 코스메틱 플랫폼 | [Code](https://github.com/SkyWith628/oblige) · [Live](https://skywith628.github.io/oblige/) |

<br>

### 🐻 [직구곰 (jikgugom)](https://github.com/SkyWith628/jikgugom)
> Amazon US → 네이버 스마트스토어 무재고 구매대행 자동화 — 돈은 결정론, 판단은 멀티에이전트

소싱→통관·인증 필터→마진→등록→발주를 잇는 **결정론 파이프라인** + 정성판단만 맡는 **3개 LLM 에이전트**의 하이브리드 설계 (셀러오션·AutoDS 벤치마킹 재설계)

- 🧠 멀티에이전트 — ① 소싱 시장성 평가 ② 한글 상세페이지 생성(DeepL+LLM) ③ CS 자동응대(민감건 결정론 에스컬레이션)
- 🛡️ **돈 판단은 LLM에 위임 안 함** — 통관·KC인증 필터, 전 비용 마진엔진, 발주 가드(매입 시 적자면 사람 승인)
- 🔌 소스/채널/저장소 **Adapter·Repository(ABC)로 추상화** — Rainforest→PA-API, 네이버→쿠팡, SQLite→PostgreSQL 교체 가능
- 🖥️ **어드민 대시보드**(Next.js 16 + FastAPI) — 승인 큐·발주 가드·재고/가격 점검 + APScheduler 주기 모니터링
- ✅ pytest 115개 · **API 키 없이 전체 동작**(mock 모드 자동 전환)

**🔧 기술적 도전 → 해결**
- **문제:** 구매대행은 잘못 등록하면 *적자 매입·통관 불가 상품 등록*처럼 돈·법적 사고로 직결된다. 그렇다고 전부 수작업이면 자동화의 의미가 없다.
- **고민:** 어디까지 LLM에 맡기고, 어디부터 막을 것인가. LLM이 환각으로 마진을 잘못 계산하면 그대로 손실이다.
- **해결:** **신뢰 경계를 "돈/규제"에 그었다.** 마진·통관·KC인증 결정은 전부 결정론 코드(마진엔진·필터·발주 가드)로 못 박고, LLM은 *시장성 판단·콘텐츠 생성·CS 응대* 같은 정성 영역만 담당. 매입 시 적자면 사람이 승인하는 HITL 가드로 마지막 안전장치를 뒀다.

**Tech:** Python · FastAPI · Claude (Anthropic) · Next.js 16 / React 19 · SQLAlchemy · APScheduler · 네이버 커머스 API

<br>

### 🧙 [CodeSage — AI 코드리뷰 에이전트](https://github.com/SkyWith628/codesage)
> Your AI pair-reviewer on every PR — GitHub PR을 자동 분석해 인라인 리뷰를 다는 LLM 에이전트 (CodeRabbit 벤치마킹)

Webhook → Redis Queue → Worker 6단계 리뷰 엔진으로 **수신과 처리를 분리**한 비동기 코드리뷰 봇

- 🔍 PR 요약 + 버그/보안/성능 **인라인 리뷰** + ruff 정적분석 통합, 증분 리뷰(새 커밋만)
- 🔐 **GitHub App 설치 토큰 자동 발급**(JWT→1시간 토큰·캐싱) + Webhook HMAC 서명 검증 + 프롬프트 인젝션 방어
- 💬 코멘트에 `@codesage` 멘션 시 diff 컨텍스트로 답하는 **대화형 후속**
- ✅ pytest 55개 · Docker Compose 풀스택(api·worker·redis·postgres) · 실제 GitHub PR 자동 리뷰 검증

**🔧 기술적 도전 → 해결**
- **문제:** GitHub Webhook은 응답이 늦으면 타임아웃 처리된다. 큰 PR을 동기로 리뷰하면 LLM 호출 시간 때문에 실패한다.
- **고민:** 매 커밋마다 PR 전체를 다시 리뷰하면 토큰 낭비·중복 코멘트가 생기고, PR 본문에 악성 지시가 섞이면 프롬프트 인젝션 위험도 있다.
- **해결:** **수신과 처리를 큐로 분리**했다. Webhook은 검증 후 즉시 200을 반환하고 실제 리뷰는 Worker가 비동기로 처리. 새 커밋 diff만 보는 **증분 리뷰**로 중복·비용을 줄이고, HMAC 서명 검증 + 인젝션 방어로 신뢰 경계를 지켰다.

**Tech:** Python · FastAPI · Google Gemini (google-genai) · Redis · PostgreSQL · Docker · pytest

<br>

### 💎 [CHECKMATE — 주얼리 이커머스](https://github.com/SkyWith628/checkmate)
> Firebase 정적 사이트를 Next.js 16 + Supabase로 전면 재설계한 풀스택 이커머스 · [Live](https://checkmate-virid-kappa.vercel.app)

카탈로그·장바구니·주문·결제·마이페이지·관리자 백오피스까지 갖춘 **풀스택 주얼리 쇼핑몰**

- 🔒 신뢰 경계를 서버에 둔 설계 — 가격·재고·쿠폰을 `place_order` RPC(security definer)에서 **원자적으로 확정**
- 🛡️ Postgres **RLS 53개 정책 + 트리거**로 접근 제어·리뷰 인증 위조 방지
- 🎨 3D 럭셔리 UI — 스크롤 reveal·포인터 틸트·glass (순수 CSS + IntersectionObserver)

**🔧 기술적 도전 → 해결**
- **문제:** 기존 Firebase 정적 구조는 가격·재고·쿠폰을 클라이언트가 다뤄, 주문 금액을 조작할 여지가 있었다.
- **고민:** 동시 주문이 몰리면 재고가 음수가 되거나 쿠폰이 중복 사용될 수 있다 — 어디서 정합성을 보장할 것인가.
- **해결:** **신뢰 경계를 DB 서버로 내렸다.** 주문 확정 로직을 `place_order` RPC(security definer) 한 곳에 모아 가격·재고·쿠폰을 원자적으로 처리하고, RLS 53개 정책과 트리거로 클라이언트가 우회할 수 없게 막았다.

**Tech:** Next.js 16 · React 19 · TypeScript · Supabase (Postgres·Auth·RLS·RPC) · Tailwind v4 · Vercel

<br>

### ♻️ [OBLIGE — Responsible Beauty](https://skywith628.github.io/oblige/) `🚧 개발 중`
> 공병을 반납하고, 지속가능한 아름다움을 채우다. · [Live](https://skywith628.github.io/oblige/)

비건 화장품 구매 · 공병 반납 · 포인트 적립 · 리필 보상까지 연결된 **ESG 코스메틱 플랫폼** (현재 개발 진행 중)

- ♻️ 공병 반납 → 포인트 적립 → 등급 상승(🌱 Seed → 🍃 Leaf → 🌳 Tree → 🌲 Forest)의 순환 구조
- 🛠️ **관리자 대시보드** — 상품·주문·공병반납·회원 통합 관리
- 🎯 **지향점:** 적립·등급 같은 핵심 로직은 Supabase(PostgreSQL)에 두어 신뢰 경계를 서버에 유지하는 것을 목표로 설계 중

**Tech:** Vanilla JS · Supabase (PostgreSQL + Auth + Storage) · GitHub Pages

---

## 🧪 서브 프로젝트

| 프로젝트 | 분야 | 한 줄 | 코드 / 데모 |
| --- | --- | --- | --- |
| 🤖 **JABIS** | AI · 데스크탑 | 음성 대화 AI 학습 비서 (Electron) — STT/TTS·학습플랜 자동생성·JARVIS 홀로그램 UI | [Code](https://github.com/SkyWith628/jabis) |
| 📊 **환율-코스피 분석** | 데이터 · 논문 | 환율이 코스피에 미치는 영향 — 상관·회귀·PCA·ARIMA + KIPS 학술발표 논문 | [Code](https://github.com/SkyWith628/kospi-exchange-analysis) |
| 📱 **iOS Practice** | 모바일 | Swift·UIKit 앱 실습 모음 (BMI·환율계산기 등) | [Code](https://github.com/SkyWith628/ios-practice) |
| 🎮 **Unity Game Basics** | 게임 | Unity·C# 입문 — 충돌·태그·GameManager 패턴 | [Code](https://github.com/SkyWith628/unity-game-basics) |
| ✅ **Todo List** | 프론트엔드 | React + TS 할 일 관리 앱 — 필터링·통계 대시보드·Custom Hook | [Code](https://github.com/SkyWith628/todo-list) · [Live](https://skywith628.github.io/todo-list/) |
| 🐍 **Data Analysis** | 데이터 | pandas·numpy·시각화 chapter별 실습 노트북 | [Code](https://github.com/SkyWith628/Data_Analysis_Python) |

---

## 📌 Tech Stack

### 🖥️ Frontend
![HTML5](https://img.shields.io/badge/-HTML5-E34F26?style=flat&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/-CSS3-1572B6?style=flat&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/-JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)
![TypeScript](https://img.shields.io/badge/-TypeScript-3178C6?style=flat&logo=typescript&logoColor=white)
![React](https://img.shields.io/badge/-React-20232A?style=flat&logo=react&logoColor=61DAFB)
![Next.js](https://img.shields.io/badge/-Next.js-000000?style=flat&logo=nextdotjs&logoColor=white)
![Electron](https://img.shields.io/badge/-Electron-47848F?style=flat&logo=electron&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/-Tailwind%20CSS-06B6D4?style=flat&logo=tailwindcss&logoColor=white)
![Vite](https://img.shields.io/badge/-Vite-646CFF?style=flat&logo=vite&logoColor=white)

### 🗄️ Backend & Database
![Python](https://img.shields.io/badge/-Python-3776AB?style=flat&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/-FastAPI-009688?style=flat&logo=fastapi&logoColor=white)
![PHP](https://img.shields.io/badge/-PHP-777BB4?style=flat&logo=php&logoColor=white)
![Laravel](https://img.shields.io/badge/-Laravel-FF2D20?style=flat&logo=laravel&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/-PostgreSQL-4169E1?style=flat&logo=postgresql&logoColor=white)
![MySQL](https://img.shields.io/badge/-MySQL-4479A1?style=flat&logo=mysql&logoColor=white)
![Supabase](https://img.shields.io/badge/-Supabase-3ECF8E?style=flat&logo=supabase&logoColor=white)
![Redis](https://img.shields.io/badge/-Redis-DC382D?style=flat&logo=redis&logoColor=white)
![SQLite](https://img.shields.io/badge/-SQLite-003B57?style=flat&logo=sqlite&logoColor=white)

### 🤖 AI / ML
![Anthropic Claude](https://img.shields.io/badge/-Claude%20(Anthropic)-D97757?style=flat&logo=anthropic&logoColor=white)
![Google Gemini](https://img.shields.io/badge/-Google%20Gemini-4285F4?style=flat&logo=google&logoColor=white)
![OpenAI](https://img.shields.io/badge/-OpenAI-412991?style=flat&logo=openai&logoColor=white)
![pandas](https://img.shields.io/badge/-pandas-150458?style=flat&logo=pandas&logoColor=white)
![scikit-learn](https://img.shields.io/badge/-scikit--learn-F7931E?style=flat&logo=scikitlearn&logoColor=white)
![statsmodels](https://img.shields.io/badge/-statsmodels-8CAAE6?style=flat)

### 🚀 DevOps & Deployment
![Docker](https://img.shields.io/badge/-Docker-2496ED?style=flat&logo=docker&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/-GitHub%20Actions-2088FF?style=flat&logo=github-actions&logoColor=white)
![Vercel](https://img.shields.io/badge/-Vercel-000000?style=flat&logo=vercel&logoColor=white)
![AWS](https://img.shields.io/badge/-AWS-FF9900?style=flat&logo=amazon-aws&logoColor=white)
![Linux](https://img.shields.io/badge/-Linux-FCC624?style=flat&logo=linux&logoColor=black)
![GitHub Pages](https://img.shields.io/badge/-GitHub%20Pages-222222?style=flat&logo=github&logoColor=white)

### 🎨 Tools
![Git](https://img.shields.io/badge/-Git-F05032?style=flat&logo=git&logoColor=white)
![Figma](https://img.shields.io/badge/-Figma-F24E1E?style=flat&logo=figma&logoColor=white)

### 📱 Mobile & Game
![Swift](https://img.shields.io/badge/-Swift-FA7343?style=flat&logo=swift&logoColor=white)
![UIKit](https://img.shields.io/badge/-UIKit-2396F3?style=flat&logo=apple&logoColor=white)
![Unity](https://img.shields.io/badge/-Unity-000000?style=flat&logo=unity&logoColor=white)
![C#](https://img.shields.io/badge/-C%23-239120?style=flat&logo=csharp&logoColor=white)

> *학습·수업 경험: C++ / Apache Spark*

---

## 🎓 경험

### 🌱 인턴십
- **2025년** 해외 의류 쇼핑몰 프론트엔드 개발 (2025.03 ~ 2025.06)

### 👋 리더십 역할
- **2024 글로벌인덕리더스쿨 팀장**
- **iOS 전공 멘토**
- **38대 총학생회 기획부장**

---

## 🎯 커리어 목표
**신뢰할 수 있는 자동화를 설계하는 AI·백엔드 엔지니어**로 성장하겠습니다.
돈·인증처럼 틀리면 안 되는 결정은 결정론으로 막고, 판단이 필요한 영역만 LLM에 맡기는 —
**견고하면서도 똑똑한 시스템**을 만드는 개발자가 되는 것이 목표입니다.

---

## 🧾 기타 정보
- 🎓 인덕대학교 컴퓨터소프트웨어 전공 재학중
- 🧾 정보처리기능사
- 🗣️ 한국어(모국어), 영어
- 🏃 취미: 러닝

---

## 🌏 소통하기
[![Blog](https://img.shields.io/badge/Blog-hskywith.tistory.com-orange?style=flat&logo=tistory)](https://hskywith.tistory.com)
[![Portfolio](https://img.shields.io/badge/Portfolio-Notion-black?style=flat&logo=notion)](https://www.notion.so/f089caa5141d414196d7170b2d8c1ed6?pvs=12)
[![GitHub](https://img.shields.io/badge/GitHub-SkyWith628-181717?style=flat&logo=github)](https://github.com/SkyWith628)

<br>
방문해 주셔서 감사합니다! 🚀
