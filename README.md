<div align="center">

![header](https://capsule-render.vercel.app/api?type=waving&color=auto&height=200&section=header&text=SkyWith&fontSize=50&animation=fadeIn&fontAlignY=38&desc=AI%20%C2%B7%20Backend%20Engineer&descAlignY=51&descAlign=62)

**LLM 에이전트와 백엔드를 만드는 신입 개발자**
*돈·인증처럼 틀리면 안 되는 결정은 결정론 코드로, 판단이 필요한 영역만 LLM으로 — 그리고 **평가로 증명**합니다.*

`Python · FastAPI` · `LangGraph · Claude / Gemini` · `PostgreSQL (pgvector) · Redis` · `Docker · GitHub Actions · AWS ECS`

인덕대학교 컴퓨터소프트웨어 전공 · 26학년도 학과 학회장

[![Blog](https://img.shields.io/badge/Blog-hskywith.tistory.com-orange?style=flat&logo=tistory)](https://hskywith.tistory.com)
[![Portfolio](https://img.shields.io/badge/Portfolio-Notion-black?style=flat&logo=notion)](https://www.notion.so/f089caa5141d414196d7170b2d8c1ed6?pvs=12)

</div>

---

## 🔥 대표 프로젝트

| 프로젝트 | 분야 | 시기 | 역할 | 한 줄 | 코드 / 데모 |
| --- | --- | --- | --- | --- | --- |
| 🗣️ **Persona_ize** | AI · 백엔드 | 2026.11 ~ 진행 중 | 팀장 (4인) | 상대별 말투로 메시지를 첨삭하되 원문의 사실·의도는 보존 | 🔒 공모전 진행 중 (비공개) |
| 🐻 **직구곰** | AI · 백엔드 | 2026.04 ~ 06 | 개인 | 구매대행 자동화 — 결정론 파이프라인 + 멀티에이전트 | [Code](https://github.com/SkyWith628/jikgugom) |
| 🧙 **CodeSage** | AI · 백엔드 | 2026.02 ~ 04 | 개인 | GitHub PR 자동 코드리뷰 에이전트 (비동기 큐) | [Code](https://github.com/SkyWith628/codesage) |
| 💎 **CHECKMATE** | 풀스택 웹 | 2026.03 ~ 05 | 개인 | 주얼리 이커머스 (Next.js + Supabase RLS) | [Code](https://github.com/SkyWith628/checkmate) · [Live](https://checkmate-virid-kappa.vercel.app) |

<br>

### 🗣️ Persona_ize — 내 말투를 지키는 메시지 첨삭 `🔒 2026.12 공모전 출품작`
> 상대·상황에 맞게 **사용자 본인의 말투**로 메시지를 고쳐 주되, 원문의 사실과 의도는 바꾸지 않는 서비스. 4인 팀(개발 1 · 기획·평가 3)의 **팀장이자 유일한 개발자**로 백엔드·워크플로·실험 하니스·배포를 맡았다.

```
초안 → 의도·보존 항목 추출 → 검색(페르소나 / 본인 과거 표현 / 그래프 맥락) → Claude 첨삭(구조화 출력)
     → 규칙 검증 → 실패 시 1회 재생성 → 재실패 시 원문 유지 → 사용자가 적용·수정·취소 (자동 전송 없음)
```

- 🧪 **"어떤 맥락이 첨삭을 낫게 하는가"를 실험으로 비교** — A 일반 / B +상대별 페르소나 / C +벡터 검색한 본인 표현(pgvector) / D +지식그래프 맥락(Neo4j·Graphiti)
- 📏 **실측:** 평가 16사례 × 4조건 × 3회 = 192회 중 190회 추천, 원문 유지 2건 → 실패 원인이 된 검증 규칙 2개를 고친 뒤 해당 사례 36회 전부 통과
- 🛡️ LLM 출력은 **규칙 검증기를 거쳐야만** 사용자에게 노출 — 통과 못 하면 원문을 그대로 돌려준다
- ✅ 백엔드 테스트 1,564개 · 화면 테스트 227개 · CI(백엔드 + iOS 빌드) · Caddy HTTPS + 백업·스모크 테스트를 포함한 Docker Compose 배포
- 🤝 비전공 팀원과의 접점을 **"파일 + PR"** 로 설계 — 팀원이 사례·루브릭 CSV를 PR로 올리면 팀장이 리뷰 후 코드에 연결

**Tech:** Python · FastAPI · LangGraph · Claude (Anthropic) · PostgreSQL + pgvector · Neo4j (Graphiti) · Docker · Caddy · SwiftUI

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
> 정적 Firebase 사이트를 Next.js 16 + Supabase로 전면 재설계한 풀스택 쇼핑몰 — 카탈로그·장바구니·주문·결제·마이페이지·관리자 백오피스 · [Live](https://checkmate-virid-kappa.vercel.app)

- 🔒 가격·재고·쿠폰을 `place_order` RPC(security definer) 한 곳에서 **원자적으로 확정**
- 🛡️ Postgres **RLS 53개 정책 + 트리거**로 접근 제어 · 리뷰 구매 인증 위조 방지
- 🎨 3D 럭셔리 UI — 스크롤 reveal·포인터 틸트·glass (순수 CSS + IntersectionObserver)

> **💡 핵심 결정** — 기존 구조는 클라이언트가 가격·재고·쿠폰을 다뤄 주문 금액 조작과 동시 주문 시 재고 음수·쿠폰 중복 사용 여지가 있었다. **신뢰 경계를 DB 서버로 내려** 클라이언트가 우회할 수 없게 막았다.

**Tech:** Next.js 16 · React 19 · TypeScript · Supabase (Postgres·Auth·RLS·RPC) · Tailwind v4 · Vercel

---

## 🤝 팀 프로젝트

### 📡 [경쟁사 모니터링 Agent](https://github.com/SkyWith628/competitor-monitoring-agent) · 2026.11 · 4인 팀
> SPA 브랜드 4곳의 상품·가격·뉴스·SNS를 수집해 변화를 감지하고, LLM으로 주간 인사이트를 만들어 슬랙·메일로 보내는 에이전트.

**내가 맡은 부분 — D (PM · 플랫폼·통합)**
- 🧱 `수집 → RawItem → ChangeEvent → 분류·요약 → WeeklyReport → 발송` 데이터 계약을 `app/core/`로 정의하고, 4명이 **자기 폴더만 고치는** 구조로 역할 분리
- ⚙️ 저장소(repo 함수 경유만 허용)·슬랙/메일 렌더링·CLI(`doctor`·`status`·`seed`) · GitHub Actions(CI, 일간 수집, 주간 리포트)
- 🔐 협업 체계 — CODEOWNERS 자동 리뷰어 지정, `main` 보호 규칙(PR 필수 · 코드 오너 승인 · 새 커밋 재승인 · ruff + pytest 통과)
- 🖥️ 팀원 전용 대시보드·리포트 열람 웹 (Next.js → Vercel)

**Tech:** Python · Naver/Google 검색 API · LLM 분류(골든셋 평가) · SQLite → PostgreSQL · GitHub Actions · Next.js

---

## 🧪 그 외 프로젝트

| 프로젝트 | 분야 | 시기 | 한 줄 | 코드 / 데모 |
| --- | --- | --- | --- | --- |
| 🔗 **Snip (URL 단축)** | 백엔드 · 인프라 | 2026.11 | FastAPI + MySQL + Nginx 3컨테이너를 **AWS ECS Fargate**에 배포 — 동시 생성 20건 충돌 0, 대소문자 콜레이션·리다이렉트 캐시 트러블슈팅 | 🔒 비공개 |
| 📈 **toss-llm-trader** | AI · 실험 | 2026.08 | LLM에 매매 전권을 주고 **모멘텀 규칙·Buy&Hold와 같은 백테스트 환경에서 비교** — point-in-time 누수 방지, tool-calling, 프롬프트 캐싱 | 🔒 비공개 |
| 🩺 **케어노트AI** | AI · 기획 | 2026.10 | 요양보호사 음성 → 방문요양 기록지 초안 (2026 지능정보 SW 아이디어 경진대회) — "말하지 않은 것은 기록하지 않는다" | 🔒 비공개 |
| ♻️ **OBLIGE** | 풀스택 웹 | 2026.05 ~ 06 | 공병 반납 → 포인트 → 등급의 ESG 코스메틱 플랫폼 + 관리자 대시보드 | [Code](https://github.com/SkyWith628/oblige) · [Live](https://skywith628.github.io/oblige/) |
| 🤖 **JABIS** | AI · 데스크탑 | 2026.03 | 음성 대화 AI 학습 비서 (Electron) — STT/TTS·학습플랜 자동생성 | [Code](https://github.com/SkyWith628/jabis) |
| ✅ **Todo List** | 프론트엔드 | 2026.01 | React + TS 할 일 관리 — 필터링·통계 대시보드·Custom Hook | [Code](https://github.com/SkyWith628/todo-list) · [Live](https://skywith628.github.io/todo-list/) |
| 📊 **환율-코스피 분석** | 데이터 · 논문 | 2025.09 ~ 12 | 상관·회귀·PCA·ARIMA 분석 + **KIPS 학술발표 논문** | [Code](https://github.com/SkyWith628/kospi-exchange-analysis) |
| 🛒 **shop69** | 백엔드 | 학부 과정 | 프레임워크 없이 PHP + MySQL로 만든 쇼핑몰 + 관리자 백오피스 (→ CHECKMATE로 재설계) | 🔒 비공개 |
| 📱 **iOS Practice** | 모바일 | 2025.09 ~ 12 | Swift·UIKit 앱 실습 (BMI·환율계산기 등) | [Code](https://github.com/SkyWith628/ios-practice) |
| 🐍 **Data Analysis** | 데이터 | 2025.09 ~ 12 | pandas·numpy·시각화 실습 노트북 | [Code](https://github.com/SkyWith628/Data_Analysis_Python) |
| 🎮 **Unity Game Basics** | 게임 | 2024.09 ~ 12 | Unity·C# 입문 — 충돌·태그·GameManager 패턴 | [Code](https://github.com/SkyWith628/unity-game-basics) |

---

## 📌 기술 스택 — 직접 해 본 것과 근거

> 기술 이름 대신 **무엇을 해 봤는지**와 **어디서 확인할 수 있는지**를 적었습니다.

**🗄️ Backend · API** — Python · FastAPI
- Webhook을 받으면 즉시 200을 반환하고, 처리는 Redis 큐 Worker가 비동기로 → **CodeSage**
- Webhook HMAC 서명 검증, GitHub App JWT로 1시간짜리 설치 토큰을 발급하고 캐싱 → **CodeSage**
- `Idempotency-Key` 헤더로 중복 요청 방지, `expected_version`으로 낙관적 잠금 → **Persona_ize**
- 외부 API·채널·저장소를 Adapter / Repository(ABC)로 추상화해 교체 가능하게 설계 → **직구곰**

**🧮 Database** — PostgreSQL · MySQL · Redis · SQLite · Neo4j
- Postgres RLS 정책 53개와 security definer RPC로 가격·재고·쿠폰을 한 번에 원자적으로 확정 → **CHECKMATE**
- UNIQUE 제약과 `ascii_bin` 콜레이션으로 단축 코드 중복·대소문자 충돌 방지, 동시 생성 20건으로 검증 → **Snip**
- pgvector 벡터 검색과 Neo4j(Graphiti) 그래프 검색을 같은 첨삭 파이프라인에 연결 → **Persona_ize**

**🧠 LLM · Agent** — Claude · Gemini · LangGraph
- LangGraph 워크플로: 추출 → 검색 → 구조화 출력 → 규칙 검증 → 1회 재생성 → 실패 시 원문 유지 → **Persona_ize**
- 돈·규제 판단은 결정론 코드로, 정성 판단만 LLM으로. 적자 매입은 사람이 승인(HITL) → **직구곰**
- 강제 tool-calling으로 결정을 구조화하고, 프롬프트 캐싱으로 호출 비용 절감 → **toss-llm-trader**
- PR 본문에 섞인 지시를 막는 프롬프트 인젝션 방어 → **CodeSage**

**📏 LLM 평가 · 실험**
- 4개 조건 × 16사례 × 3회 반복 실험 하니스 → 실패 원인이 된 규칙 수정 → 재측정 → **Persona_ize**
- point-in-time 시계로 백테스트의 미래 정보 누수를 막고, 규칙 전략·Buy&Hold와 같은 환경에서 비교 → **toss-llm-trader**

**✅ 테스트 · 협업**
- pytest — Persona_ize 1,564개 · 직구곰 115개 · CodeSage 55개. API 키 없이 mock·fake 모델로 전체 실행 → **직구곰 · Persona_ize**
- ruff + pytest CI, `main` 보호 규칙(PR 필수 · 코드 오너 승인 · 새 커밋 재승인), CODEOWNERS 자동 리뷰어 지정 → **경쟁사 모니터링**

**🚀 Infra · 배포** — Docker · GitHub Actions · AWS ECS · Nginx · Caddy · Vercel
- 컨테이너 3개를 AWS ECS Fargate에 배포하고 외부에는 80번 포트만 노출, 멀티스테이지 빌드(frontend 73.7MB) → **Snip**
- ECS 태스크가 뜨기 전에 멈추는 `ResourceInitializationError`의 원인을 실행 역할의 로그 권한 누락으로 찾아 해결 → **Snip**
- Caddy 자동 HTTPS · 주기 백업 · 배포 후 스모크 테스트를 묶은 배포 스크립트 → **Persona_ize**
- GitHub Actions로 일간 수집과 주간 리포트 자동 실행 → **경쟁사 모니터링**

**🖥️ Frontend · App** — TypeScript · React · Next.js · SwiftUI
- Next.js 16 + Supabase Auth 풀스택 쇼핑몰, 관리자 백오피스 → **CHECKMATE**
- SwiftUI 아이폰 앱과 보조 키보드 확장, CI에서 시뮬레이터 빌드 → **Persona_ize**

<sub>그 외 사용 경험: PHP(shop69) · Electron(JABIS) · pandas·statsmodels(환율-코스피 분석) · UIKit(iOS Practice) · Unity/C#(Unity Game Basics)</sub>

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

방문해 주셔서 감사합니다! 🚀

</div>
