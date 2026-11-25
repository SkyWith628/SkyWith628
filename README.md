![banner](https://capsule-render.vercel.app/api?type=waving&color=0:A8C0D8,100:5B6B7F&height=140&section=header)

<div align="center">

# SkyWith

**LLM을 실제 서비스에 붙이는 백엔드 개발자**<br>
<sub>틀리면 안 되는 결정은 코드로, LLM의 출력은 검증을 통과해야 내보냅니다</sub>

<br>

<img src="https://skillicons.dev/icons?i=py,fastapi,postgres,redis,docker,githubactions,aws,nextjs,ts&theme=light" alt="Python, FastAPI, PostgreSQL, Redis, Docker, GitHub Actions, AWS, Next.js, TypeScript" />

<br><br>

<a href="https://hskywith.tistory.com"><img src="https://img.shields.io/badge/Blog-tistory-555555?style=flat-square" alt="Blog" /></a>
<a href="https://www.notion.so/f089caa5141d414196d7170b2d8c1ed6?pvs=12"><img src="https://img.shields.io/badge/Portfolio-Notion-555555?style=flat-square&logo=notion&logoColor=white" alt="Portfolio" /></a>

</div>

<br>

프로젝트를 몇 번 하면서 생긴 기준이 하나 있습니다. LLM을 썼다는 사실보다, 그 LLM을 어떻게 검증하고 통제했는지가 더 중요하다는 것입니다.

- **통제**: 돈이나 인증처럼 틀리면 곤란한 결정은 코드로 정하고, LLM에는 사람이 판단하던 부분만 맡깁니다. 위험한 건 사람 승인을 거칩니다. → [직구곰](#직구곰)
- **검증**: LLM이 낸 결과는 규칙 검사를 통과해야만 사용자에게 나갑니다. 실패하면 다시 만들고, 그래도 안 되면 원문으로 돌아갑니다. → [Persona_ize](#persona_ize)
- **측정**: 정말 나아졌는지는 감이 아니라 조건을 바꿔 가며 돌려 보고 숫자로 확인합니다. → [Persona_ize](#persona_ize) (4조건 × 192회 실험)

LLM 쪽은 LangGraph와 Claude, Gemini를 씁니다. 인덕대학교 컴퓨터소프트웨어학과에 다니고 있습니다.

---

## 대표 프로젝트

### [Persona_ize](https://github.com/SkyWith628/persona-ize)
<sub>2026.11 ~ 진행 중 · 4인 팀, 팀장 · 2026.12 공모전 출품작</sub>

> 상대에 맞게 메시지를 다듬어 주되, 내 말투와 원래 내용은 그대로 두는 서비스

상사에게 보내는 말과 친구에게 보내는 말은 다르지만, 내가 하려던 말까지 바뀌면 안 됩니다. 그래서 "고쳐 쓰되 사실과 의도는 건드리지 않는다"를 가장 먼저 정했습니다. 팀원 셋이 비전공자라 개발은 제가 전부 맡았고, 팀원들은 평가 사례와 루브릭을 CSV로 만들어 PR로 올렸습니다.

- **검증**: 첨삭 결과는 규칙 검증을 통과해야만 보입니다. 실패하면 한 번 다시 만들고, 그래도 안 되면 원문을 돌려줍니다.
- **실험**: 어떤 정보를 넣어야 첨삭이 나아지는지 네 조건(없음 / 상대별 페르소나 / 내 과거 표현 검색 pgvector / 대화 맥락 그래프 Neo4j)으로 비교했습니다. 16개 사례 × 4조건 × 3회 = 192회 중 190회 통과. 실패를 보고 검증 규칙 두 개를 고친 뒤 문제 사례 3개를 36회 다시 돌려 모두 통과했습니다.
- **운영**: 백엔드 테스트 1,564개, 화면 테스트 227개. Docker Compose + Caddy(HTTPS)로 배포하고 백업·배포 후 점검 스크립트를 만들었습니다. 아이폰 앱(SwiftUI)은 CI에서 빌드합니다.

`FastAPI` `LangGraph` `Claude` `PostgreSQL + pgvector` `Neo4j` `Docker` `SwiftUI`

<br>

### [경쟁사 모니터링 Agent](https://github.com/SkyWith628/competitor-monitoring-agent)
<sub>2026.11 · 4인 팀, 플랫폼·PM</sub>

> SPA 브랜드 네 곳의 상품, 가격, 뉴스, SNS를 모아 매주 변화를 정리해 슬랙과 메일로 보내 주는 에이전트

넷이서 사이트 수집, 뉴스·SNS 수집, 분석, 플랫폼을 하나씩 맡았고, 저는 플랫폼과 PM을 맡았습니다. 넷이 동시에 작업해도 서로 코드를 덮어쓰지 않게 만드는 게 제 일이었습니다.

- **분업**: 수집부터 발송까지 단계마다 주고받는 데이터 형식을 먼저 정하고, 각자 자기 폴더만 고치도록 나눴습니다.
- **플랫폼**: 저장소, 슬랙·메일 발송, CLI, 팀원용 대시보드(Next.js)를 만들었습니다.
- **규칙**: CODEOWNERS로 리뷰어를 자동 지정하고, main은 PR·담당자 승인·CI(ruff + pytest) 통과 없이 합칠 수 없게 막았습니다. 매일 수집과 주간 리포트는 GitHub Actions로 돌아갑니다.

`Python` `GitHub Actions` `PostgreSQL` `Next.js`

<br>

### [직구곰](https://github.com/SkyWith628/jikgugom)
<sub>2026.04 ~ 06 · 개인</sub>

> AliExpress·Amazon 상품을 네이버·쿠팡에 올려 파는 구매대행을 자동화한 프로젝트

구매대행은 상품 하나를 잘못 올리면 바로 손해가 납니다. 마진 계산을 틀리거나 통관이 안 되는 상품, KC 인증 대상 상품을 놓치면 돈이나 법 문제로 이어지기 때문에, 이런 판단은 LLM에 맡기지 않기로 했습니다.

- **통제**: 소싱부터 통관·인증 필터, 마진 계산, 등록, 발주까지는 일반 코드로 짰고, 사는 순간 적자가 되는 주문은 사람이 승인해야 넘어갑니다. LLM은 상품성 평가, 한글 상세페이지 작성(DeepL 번역 후 다듬기), 고객 문의 응대 세 곳에만 쓰고, 민감한 문의는 사람에게 넘깁니다.
- **구조**: 상품 출처(AliExpress, Amazon), 판매 채널(네이버, 쿠팡), 저장소를 인터페이스로 분리했습니다. 채널을 추가하거나 SQLite를 PostgreSQL로 바꿔도 나머지 코드는 그대로입니다.
- **운영**: pytest 194개. API 키가 없어도 mock 모드로 전체 흐름이 돌아가고, 관리자 대시보드(Next.js)에서 승인과 발주를 처리합니다.

`Python` `FastAPI` `Gemini` `Next.js` `SQLAlchemy` `APScheduler`

<br>

### [CodeSage](https://github.com/SkyWith628/codesage)
<sub>2026.02 ~ 04 · 개인</sub>

> GitHub PR이 올라오면 자동으로 코드 리뷰를 달아 주는 봇

GitHub Webhook은 응답이 늦으면 실패로 처리하는데, PR이 조금만 커도 LLM 호출이 그 시간을 넘깁니다. 그래서 Webhook은 서명만 확인하고 바로 응답하고, 실제 리뷰는 Redis 큐 뒤의 Worker가 따로 처리하도록 나눴습니다.

- **리뷰**: 새 커밋이 올라오면 바뀐 부분만 다시 봐서, 같은 지적이 반복되지 않고 토큰도 덜 씁니다. 코멘트에서 `@codesage`를 부르면 해당 diff를 보고 답합니다.
- **보안**: GitHub App 설치 토큰을 JWT로 발급받아 캐싱하고, Webhook은 HMAC 서명으로 검증합니다. PR 본문에 숨긴 지시문에 끌려가지 않도록 프롬프트도 막아 뒀습니다.
- **운영**: pytest 55개. Docker Compose로 api, worker, redis, postgres를 한 번에 띄웁니다.

`Python` `FastAPI` `Gemini` `Redis` `PostgreSQL` `Docker`

<br>

### [CHECKMATE](https://github.com/SkyWith628/checkmate)
<sub>2026.03 ~ 05 · 개인 · [사이트](https://checkmate-virid-kappa.vercel.app)</sub>

> 주얼리 쇼핑몰을 Next.js와 Supabase로 다시 만든 프로젝트

원래는 Firebase로 만든 정적 사이트였는데, 가격과 쿠폰 계산을 브라우저에서 하고 있어서 결제 금액을 바꿀 여지가 있었습니다. 그래서 주문 금액을 정하는 일을 전부 DB로 옮겼습니다.

- **정합성**: 주문 확정은 `place_order`라는 DB 함수 하나에서만 일어납니다. 가격, 재고, 쿠폰을 한 트랜잭션 안에서 확인해서 주문이 몰려도 재고가 음수로 내려가지 않습니다.
- **권한**: Postgres RLS 정책 53개와 트리거로 남의 주문을 보거나, 사지 않은 상품에 리뷰를 다는 걸 막았습니다.
- **범위**: 장바구니, 주문, 결제, 마이페이지, 관리자 페이지까지 쇼핑몰에 필요한 화면을 모두 만들었습니다.

`Next.js` `TypeScript` `Supabase` `PostgreSQL` `Tailwind` `Vercel`

---

## 그 밖에 만든 것들

| 프로젝트 | 시기 | 내용 | 링크 |
| --- | --- | --- | --- |
| Snip | 2026.11 | URL 단축 서비스. FastAPI·MySQL·Nginx 컨테이너를 AWS ECS Fargate에 배포하고, 동시 생성 시 코드 충돌과 대소문자 중복을 막았습니다. 처음엔 태스크가 뜨지도 않았는데, 로그 그룹을 만들 권한이 없어서였습니다. | [코드](https://github.com/SkyWith628/url-shortener) |
| toss-llm-trader | 2026.08 | LLM 매매가 단순 보유보다 나은지 모의투자로 재 본 실험. 백테스트가 미래 가격을 미리 보지 못하게 막았습니다. | [코드](https://github.com/SkyWith628/toss-llm-trader) |
| 케어노트AI | 2026.10 | 요양보호사의 음성 기록을 방문요양 기록지 초안으로 바꾸는 서비스 기획 (2026 지능정보 SW 아이디어 경진대회) | [코드](https://github.com/SkyWith628/carenote-ai) |
| OBLIGE | 2026.05 ~ 06 | 공병 사진을 YOLOv8로 인식해 포인트로 돌려주는 ESG 화장품 쇼핑몰. 반납 안내는 tool-use LLM 어시스턴트가 맡습니다. | [코드](https://github.com/SkyWith628/oblige) · [사이트](https://skywith628.github.io/oblige/) |
| JABIS | 2026.03 | 음성으로 대화하는 학습 도우미 데스크톱 앱 (Electron) | [코드](https://github.com/SkyWith628/jabis) |
| Todo List | 2026.01 | React·TypeScript 할 일 관리 앱 | [코드](https://github.com/SkyWith628/todo-list) · [사이트](https://skywith628.github.io/todo-list/) |
| 환율과 코스피 | 2025.09 ~ 12 | 환율이 코스피에 주는 영향을 회귀·PCA·ARIMA로 분석, KIPS 학술대회 발표 | [코드](https://github.com/SkyWith628/kospi-exchange-analysis) |
| shop69 | 학부 수업 | 프레임워크 없이 PHP·MySQL로 만든 첫 쇼핑몰. 같은 주제를 CHECKMATE로 다시 만들었습니다. | [코드](https://github.com/SkyWith628/php_shop_project) |

<sub>수업 실습: [iOS Practice](https://github.com/SkyWith628/ios-practice) (Swift·UIKit) · [Data Analysis](https://github.com/SkyWith628/Data_Analysis_Python) (pandas·numpy) · [Unity Game Basics](https://github.com/SkyWith628/unity-game-basics) (Unity·C#)</sub>

---

## 기술

| 영역 | 써 본 것 | 어디서 |
| --- | --- | --- |
| 백엔드 | FastAPI, Redis 큐, 중복 요청 한 번만 처리, 버전 확인으로 덮어쓰기 방지 | CodeSage, Persona_ize |
| 데이터베이스 | PostgreSQL 함수·RLS, pgvector, Neo4j, UNIQUE·콜레이션 | CHECKMATE, Persona_ize, Snip |
| LLM | LangGraph, Claude, Gemini, 출력 검증·재생성, 조건별 반복 실험 | Persona_ize, 직구곰, toss-llm-trader |
| 배포·협업 | Docker Compose, Caddy, AWS ECS Fargate, GitHub Actions, 브랜치 보호 | Persona_ize, Snip, 경쟁사 모니터링 |

이 밖에 PHP, Electron, Swift, Unity도 수업과 개인 작업에서 써 봤습니다.

---

## 활동

- 2026 컴퓨터소프트웨어학과 학회장
- 2026 38대 총학생회 기획부장
- 2025 iOS 전공 멘토
- 2024 글로벌인덕리더스쿨 팀장

---

<div align="center">

<sub>읽어 주셔서 감사합니다.</sub>

</div>

![footer](https://capsule-render.vercel.app/api?type=waving&color=0:A8C0D8,100:5B6B7F&height=90&section=footer)
