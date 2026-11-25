<div align="center">

![header](https://capsule-render.vercel.app/api?type=waving&color=auto&height=200&section=header&text=SkyWith&fontSize=50&animation=fadeIn&fontAlignY=38&desc=AI%20%C2%B7%20Backend%20Engineer&descAlignY=51&descAlign=62)

**신뢰할 수 있는 자동화를 설계하는 AI · 백엔드 개발자**
*돈·인증처럼 틀리면 안 되는 결정은 결정론 코드로, 판단이 필요한 영역만 LLM으로.*

인덕대학교 컴퓨터소프트웨어 전공 · 26학년도 학과 학회장

[![Blog](https://img.shields.io/badge/Blog-hskywith.tistory.com-orange?style=flat&logo=tistory)](https://hskywith.tistory.com)
[![Portfolio](https://img.shields.io/badge/Portfolio-Notion-black?style=flat&logo=notion)](https://www.notion.so/f089caa5141d414196d7170b2d8c1ed6?pvs=12)
[![GitHub](https://img.shields.io/badge/GitHub-SkyWith628-181717?style=flat&logo=github)](https://github.com/SkyWith628)

</div>

---

## 🔥 대표 프로젝트

| 프로젝트 | 분야 | 시기 | 한 줄 | 코드 / 데모 |
| --- | --- | --- | --- | --- |
| 🐻 **직구곰** | AI · 백엔드 | 2026.04 ~ 진행 중 | 구매대행 자동화 — 결정론 파이프라인 + 멀티에이전트 | [Code](https://github.com/SkyWith628/jikgugom) |
| 🧙 **CodeSage** | AI · 백엔드 | 2026.02 ~ 04 | GitHub PR 자동 코드리뷰 에이전트 (비동기 큐) | [Code](https://github.com/SkyWith628/codesage) |
| 💎 **CHECKMATE** | 풀스택 웹 | 2026.03 ~ 05 | 주얼리 이커머스 (Next.js + Supabase RLS) | [Code](https://github.com/SkyWith628/checkmate) · [Live](https://checkmate-virid-kappa.vercel.app) |
| ♻️ **OBLIGE** | 풀스택 웹 | 2026.05 ~ 진행 중 | 공병 반납 ESG 코스메틱 플랫폼 | [Code](https://github.com/SkyWith628/oblige) · [Live](https://skywith628.github.io/oblige/) |

<br>

### 🐻 [직구곰 (jikgugom)](https://github.com/SkyWith628/jikgugom)
> Amazon US → 네이버 스마트스토어 무재고 구매대행 자동화. 소싱 → 통관·인증 필터 → 마진 → 등록 → 발주를 잇는 **결정론 파이프라인**에, 정성 판단만 맡는 **LLM 에이전트 3개**를 얹은 하이브리드 설계.

- 🛡️ **돈 판단은 LLM에 맡기지 않음** — 통관·KC인증 필터, 전 비용 마진엔진, 매입 시 적자면 사람이 승인하는 발주 가드(HITL)
- 🧠 에이전트는 정성 영역만 — ① 소싱 시장성 평가 ② 한글 상세페이지 생성(DeepL + LLM) ③ CS 자동응대(민감 건은 결정론 에스컬레이션)
- 🔌 소스·채널·저장소를 **Adapter / Repository(ABC)로 추상화** — Rainforest→PA-API, 네이버→쿠팡, SQLite→PostgreSQL 교체 가능
- 🖥️ 어드민 대시보드(Next.js 16 + FastAPI) — 승인 큐·재고/가격 점검, APScheduler 주기 모니터링
- ✅ pytest 115개 · API 키 없이도 mock 모드로 전체 동작

> **💡 핵심 결정** — LLM이 환각으로 마진을 잘못 계산하면 그대로 손실이고, 통관 불가 상품을 등록하면 법적 사고다. 그래서 **신뢰 경계를 "돈·규제"에 그었다.** 결정은 코드가, 판단은 LLM이.

**Tech:** Python · FastAPI · Claude (Anthropic) · Next.js 16 / React 19 · SQLAlchemy · APScheduler · 네이버 커머스 API

<br>

### 🧙 [CodeSage — AI 코드리뷰 에이전트](https://github.com/SkyWith628/codesage)
> GitHub PR을 자동 분석해 인라인 리뷰를 다는 LLM 에이전트. Webhook → Redis Queue → Worker로 **수신과 처리를 분리**한 비동기 구조.

- 🔍 PR 요약 + 버그·보안·성능 **인라인 리뷰**, ruff 정적분석 통합, 새 커밋만 보는 **증분 리뷰**
- 🔐 GitHub App 설치 토큰 자동 발급(JWT → 1시간 토큰, 캐싱) · Webhook HMAC 서명 검증 · 프롬프트 인젝션 방어
- 💬 코멘트에서 `@codesage` 멘션 시 diff 컨텍스트로 답하는 대화형 후속
- ✅ pytest 55개 · Docker Compose(api·worker·redis·postgres) · 실제 PR 자동 리뷰 검증

> **💡 핵심 결정** — Webhook은 응답이 늦으면 타임아웃된다. 큰 PR을 동기로 리뷰하면 LLM 호출 시간 때문에 실패하므로, **즉시 200을 반환하고 리뷰는 Worker가 비동기 처리**하도록 분리했다. 증분 리뷰로 중복 코멘트와 토큰 비용도 함께 줄였다.

**Tech:** Python · FastAPI · Google Gemini (google-genai) · Redis · PostgreSQL · Docker · pytest

<br>

### 💎 [CHECKMATE — 주얼리 이커머스](https://github.com/SkyWith628/checkmate)
> Firebase 정적 사이트를 Next.js 16 + Supabase로 전면 재설계한 풀스택 쇼핑몰 — 카탈로그·장바구니·주문·결제·마이페이지·관리자 백오피스 · [Live](https://checkmate-virid-kappa.vercel.app)

- 🔒 가격·재고·쿠폰을 `place_order` RPC(security definer) 한 곳에서 **원자적으로 확정**
- 🛡️ Postgres **RLS 53개 정책 + 트리거**로 접근 제어 · 리뷰 구매 인증 위조 방지
- 🎨 3D 럭셔리 UI — 스크롤 reveal·포인터 틸트·glass (순수 CSS + IntersectionObserver)

> **💡 핵심 결정** — 기존 구조는 클라이언트가 가격·재고·쿠폰을 다뤄 주문 금액 조작과 동시 주문 시 재고 음수·쿠폰 중복 사용 여지가 있었다. **신뢰 경계를 DB 서버로 내려** 클라이언트가 우회할 수 없게 막았다.

**Tech:** Next.js 16 · React 19 · TypeScript · Supabase (Postgres·Auth·RLS·RPC) · Tailwind v4 · Vercel

<br>

### ♻️ [OBLIGE — Responsible Beauty](https://github.com/SkyWith628/oblige) `🚧 개발 중`
> 비건 화장품 구매 → 공병 반납 → 포인트 적립 → 등급 상승(🌱 Seed → 🍃 Leaf → 🌳 Tree → 🌲 Forest)을 잇는 ESG 코스메틱 플랫폼 · [Live](https://skywith628.github.io/oblige/)

- 🛠️ 관리자 대시보드 — 상품·주문·공병 반납·회원 통합 관리
- 🎯 적립·등급 로직을 Supabase(PostgreSQL)에 두어 신뢰 경계를 서버에 유지하도록 설계 중

**Tech:** Vanilla JS · Supabase (PostgreSQL + Auth + Storage) · GitHub Pages

---

## 🧪 서브 프로젝트

| 프로젝트 | 분야 | 시기 | 한 줄 | 코드 / 데모 |
| --- | --- | --- | --- | --- |
| 🤖 **JABIS** | AI · 데스크탑 | 2026.03 | 음성 대화 AI 학습 비서 (Electron) — STT/TTS·학습플랜 자동생성 | [Code](https://github.com/SkyWith628/jabis) |
| ✅ **Todo List** | 프론트엔드 | 2026.01 | React + TS 할 일 관리 — 필터링·통계 대시보드·Custom Hook | [Code](https://github.com/SkyWith628/todo-list) · [Live](https://skywith628.github.io/todo-list/) |
| 📊 **환율-코스피 분석** | 데이터 · 논문 | 2025.09 ~ 12 | 상관·회귀·PCA·ARIMA 분석 + **KIPS 학술발표 논문** | [Code](https://github.com/SkyWith628/kospi-exchange-analysis) |
| 📱 **iOS Practice** | 모바일 | 2025.09 ~ 12 | Swift·UIKit 앱 실습 (BMI·환율계산기 등) | [Code](https://github.com/SkyWith628/ios-practice) |
| 🐍 **Data Analysis** | 데이터 | 2025.09 ~ 12 | pandas·numpy·시각화 실습 노트북 | [Code](https://github.com/SkyWith628/Data_Analysis_Python) |
| 🎮 **Unity Game Basics** | 게임 | 2024.09 ~ 12 | Unity·C# 입문 — 충돌·태그·GameManager 패턴 | [Code](https://github.com/SkyWith628/unity-game-basics) |

---

## 📌 Tech Stack

**주력 — 프로젝트에서 실제로 쓴 것**

![Python](https://img.shields.io/badge/-Python-3776AB?style=flat&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/-FastAPI-009688?style=flat&logo=fastapi&logoColor=white)
![TypeScript](https://img.shields.io/badge/-TypeScript-3178C6?style=flat&logo=typescript&logoColor=white)
![Next.js](https://img.shields.io/badge/-Next.js-000000?style=flat&logo=nextdotjs&logoColor=white)
![React](https://img.shields.io/badge/-React-20232A?style=flat&logo=react&logoColor=61DAFB)
![PostgreSQL](https://img.shields.io/badge/-PostgreSQL-4169E1?style=flat&logo=postgresql&logoColor=white)
![Supabase](https://img.shields.io/badge/-Supabase-3ECF8E?style=flat&logo=supabase&logoColor=white)
![Redis](https://img.shields.io/badge/-Redis-DC382D?style=flat&logo=redis&logoColor=white)
![Docker](https://img.shields.io/badge/-Docker-2496ED?style=flat&logo=docker&logoColor=white)
![Claude](https://img.shields.io/badge/-Claude%20(Anthropic)-D97757?style=flat&logo=anthropic&logoColor=white)
![Gemini](https://img.shields.io/badge/-Google%20Gemini-4285F4?style=flat&logo=google&logoColor=white)

<details>
<summary><b>그 외 사용 경험</b></summary>
<br>

**Frontend** &nbsp;
![HTML5](https://img.shields.io/badge/-HTML5-E34F26?style=flat&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/-CSS3-1572B6?style=flat&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/-JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)
![Tailwind CSS](https://img.shields.io/badge/-Tailwind%20CSS-06B6D4?style=flat&logo=tailwindcss&logoColor=white)
![Vite](https://img.shields.io/badge/-Vite-646CFF?style=flat&logo=vite&logoColor=white)
![Electron](https://img.shields.io/badge/-Electron-47848F?style=flat&logo=electron&logoColor=white)

**Backend & DB** &nbsp;
![PHP](https://img.shields.io/badge/-PHP-777BB4?style=flat&logo=php&logoColor=white)
![Laravel](https://img.shields.io/badge/-Laravel-FF2D20?style=flat&logo=laravel&logoColor=white)
![MySQL](https://img.shields.io/badge/-MySQL-4479A1?style=flat&logo=mysql&logoColor=white)
![SQLite](https://img.shields.io/badge/-SQLite-003B57?style=flat&logo=sqlite&logoColor=white)

**AI / Data** &nbsp;
![OpenAI](https://img.shields.io/badge/-OpenAI-412991?style=flat&logo=openai&logoColor=white)
![pandas](https://img.shields.io/badge/-pandas-150458?style=flat&logo=pandas&logoColor=white)
![scikit-learn](https://img.shields.io/badge/-scikit--learn-F7931E?style=flat&logo=scikitlearn&logoColor=white)
![statsmodels](https://img.shields.io/badge/-statsmodels-8CAAE6?style=flat)

**DevOps** &nbsp;
![GitHub Actions](https://img.shields.io/badge/-GitHub%20Actions-2088FF?style=flat&logo=github-actions&logoColor=white)
![Vercel](https://img.shields.io/badge/-Vercel-000000?style=flat&logo=vercel&logoColor=white)
![AWS](https://img.shields.io/badge/-AWS-FF9900?style=flat&logo=amazon-aws&logoColor=white)
![Linux](https://img.shields.io/badge/-Linux-FCC624?style=flat&logo=linux&logoColor=black)
![GitHub Pages](https://img.shields.io/badge/-GitHub%20Pages-222222?style=flat&logo=github&logoColor=white)

**Mobile & Game** &nbsp;
![Swift](https://img.shields.io/badge/-Swift-FA7343?style=flat&logo=swift&logoColor=white)
![UIKit](https://img.shields.io/badge/-UIKit-2396F3?style=flat&logo=apple&logoColor=white)
![Unity](https://img.shields.io/badge/-Unity-000000?style=flat&logo=unity&logoColor=white)
![C#](https://img.shields.io/badge/-C%23-239120?style=flat&logo=csharp&logoColor=white)

**Tools** &nbsp;
![Git](https://img.shields.io/badge/-Git-F05032?style=flat&logo=git&logoColor=white)
![Figma](https://img.shields.io/badge/-Figma-F24E1E?style=flat&logo=figma&logoColor=white)

*수업 경험: C++ · Apache Spark*

</details>

---

## 🎓 경험 · 자격

**리더십**
- 26학년도 컴퓨터소프트웨어학과 학회장
- 38대 총학생회 기획부장
- iOS 전공 멘토
- 2024 글로벌인덕리더스쿨 팀장

**자격 · 기타**
- 🧾 정보처리기능사
- 📄 KIPS 학술발표대회 논문 발표 (환율-코스피 분석)
- 🗣️ 한국어(모국어) · 영어
- 🏃 취미: 러닝

<br>

<div align="center">

![GitHub Stats](https://github-readme-stats.vercel.app/api?username=SkyWith628&show_icons=true&hide_border=true&count_private=true&theme=tokyonight)

방문해 주셔서 감사합니다! 🚀

</div>
