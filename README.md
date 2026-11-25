<div align="center">

# 안녕하세요, SkyWith입니다

LLM을 실제 서비스에 붙이는 백엔드를 만듭니다

<br>

<img src="https://skillicons.dev/icons?i=py,fastapi,postgres,redis,docker,githubactions,aws,nextjs,ts&theme=light" alt="Python, FastAPI, PostgreSQL, Redis, Docker, GitHub Actions, AWS, Next.js, TypeScript" />

<br><br>

<a href="https://hskywith.tistory.com"><img src="https://img.shields.io/badge/Blog-tistory-555555?style=flat-square" alt="Blog" /></a>
<a href="https://www.notion.so/f089caa5141d414196d7170b2d8c1ed6?pvs=12"><img src="https://img.shields.io/badge/Portfolio-Notion-555555?style=flat-square&logo=notion&logoColor=white" alt="Portfolio" /></a>

</div>

<br>

인덕대학교 컴퓨터소프트웨어학과에 다니고 있고, 올해는 학과 학회장을 맡고 있습니다.

프로젝트를 몇 번 하면서 생긴 기준이 하나 있습니다. 돈이나 인증처럼 틀리면 곤란한 결정은 코드로 정하고, LLM에는 사람이 판단하던 부분만 맡긴다는 것입니다. 그렇게 만든 게 정말 나아졌는지는 감이 아니라 측정해서 확인하려고 합니다.

주로 쓰는 것은 Python, FastAPI, PostgreSQL, Redis, Docker이고, LLM 쪽은 LangGraph와 Claude, Gemini를 씁니다.

---

## 대표 프로젝트

### [Persona_ize](https://github.com/SkyWith628/persona-ize)
<sub>2026.11 ~ 진행 중 · 4인 팀, 팀장 · 2026.12 공모전 출품작</sub>

> 상대에 맞게 메시지를 다듬어 주되, 내 말투와 원래 내용은 그대로 두는 서비스

상사에게 보내는 말과 친구에게 보내는 말은 다르지만, 그렇다고 내가 하려던 말이 바뀌면 안 됩니다. 그래서 이 프로젝트에서는 "고쳐 쓰되 사실과 의도는 건드리지 않는다"를 가장 먼저 정했습니다.

팀원 셋이 비전공자라 개발은 제가 전부 맡았습니다. 백엔드, LangGraph 워크플로, 실험 하니스, 배포까지입니다. 팀원들은 평가 사례와 루브릭을 CSV로 만들어 PR로 올리고, 제가 리뷰해서 코드에 연결하는 식으로 일했습니다.

- 첨삭 결과는 규칙 검증을 통과해야만 사용자에게 보입니다. 실패하면 한 번 다시 만들고, 그래도 안 되면 원문을 그대로 돌려줍니다.
- 어떤 정보를 넣어야 첨삭이 나아지는지 네 가지 조건으로 비교했습니다. 아무것도 없이, 상대별 페르소나 추가, 내 과거 표현 벡터 검색(pgvector) 추가, 대화 맥락 그래프(Neo4j) 추가입니다.
- 16개 사례를 조건마다 3번씩, 총 192번 돌렸고 190번이 추천까지 갔습니다. 결과를 보고 검증 규칙 두 개를 고친 뒤, 문제가 있던 세 사례를 다시 36번 돌려 전부 통과했습니다.
- 백엔드 테스트 1,564개, 화면 테스트 227개가 있습니다. 배포는 Docker Compose에 Caddy(HTTPS)를 붙였고, 백업과 배포 후 점검 스크립트도 만들었습니다. 아이폰 앱(SwiftUI)은 CI에서 빌드합니다.

`FastAPI` `LangGraph` `Claude` `PostgreSQL + pgvector` `Neo4j` `Docker` `SwiftUI`

<br>

### [직구곰](https://github.com/SkyWith628/jikgugom)
<sub>2026.04 ~ 06 · 개인</sub>

> 아마존 상품을 네이버 스마트스토어에 올려 파는 구매대행을 자동화한 프로젝트

구매대행은 상품 하나를 잘못 올리면 바로 손해가 나는 일입니다. 마진 계산을 틀리거나, 통관이 안 되는 상품을 올리거나, KC 인증 대상인 걸 놓치면 돈이나 법 문제로 이어집니다. LLM이 한 번만 잘못 계산해도 적자 매입이 되기 때문에, 이런 판단은 LLM에 맡기지 않기로 했습니다.

- 소싱부터 통관·인증 필터, 마진 계산, 등록, 발주까지는 전부 일반 코드로 짰습니다. 사는 순간 적자가 되는 주문은 사람이 승인해야 넘어갑니다.
- LLM은 세 군데만 씁니다. 팔릴 만한 상품인지 평가, 한글 상세페이지 작성(DeepL 번역 후 다듬기), 고객 문의 응대입니다. 민감한 문의는 LLM이 답하지 않고 사람에게 넘깁니다.
- 상품 데이터 출처, 판매 채널, 저장소를 인터페이스로 분리해 뒀습니다. 나중에 쿠팡을 붙이거나 SQLite를 PostgreSQL로 바꿔도 나머지 코드는 손대지 않아도 됩니다.
- pytest 115개가 있고, API 키가 없어도 mock 모드로 전체 흐름이 돌아갑니다.

`Python` `FastAPI` `Claude` `Next.js` `SQLAlchemy` `APScheduler`

<br>

### [CodeSage](https://github.com/SkyWith628/codesage)
<sub>2026.02 ~ 04 · 개인</sub>

> GitHub PR이 올라오면 자동으로 코드 리뷰를 달아 주는 봇

GitHub Webhook은 응답이 늦으면 실패로 처리하는데, PR이 조금만 커도 LLM 호출만으로 그 시간을 넘깁니다. 그래서 요청을 받는 부분과 리뷰하는 부분을 Redis 큐로 나눴습니다. Webhook은 서명만 확인하고 바로 응답하고, 실제 리뷰는 Worker가 따로 처리합니다.

- 새 커밋이 올라오면 바뀐 부분만 다시 봅니다. 같은 지적이 반복해서 달리지 않고 토큰도 덜 씁니다.
- GitHub App 방식이라 설치 토큰을 JWT로 발급받아 캐싱해서 씁니다. Webhook은 HMAC 서명으로 검증하고, PR 본문에 숨긴 지시문에 끌려가지 않도록 프롬프트도 막아 뒀습니다.
- 코멘트에서 `@codesage`를 부르면 해당 diff를 보고 답합니다.
- pytest 55개가 있고, Docker Compose로 api, worker, redis, postgres를 한 번에 띄웁니다.

`Python` `FastAPI` `Gemini` `Redis` `PostgreSQL` `Docker`

<br>

### [CHECKMATE](https://github.com/SkyWith628/checkmate)
<sub>2026.03 ~ 05 · 개인 · [사이트](https://checkmate-virid-kappa.vercel.app)</sub>

> 주얼리 쇼핑몰을 Next.js와 Supabase로 다시 만든 프로젝트

원래는 Firebase로 만든 정적 사이트였는데, 가격과 쿠폰 계산을 브라우저에서 하고 있었습니다. 마음만 먹으면 결제 금액을 바꿀 여지가 있는 구조였습니다. 그래서 주문 금액을 정하는 일을 전부 DB로 옮겼습니다.

- 주문 확정은 `place_order`라는 DB 함수 하나에서만 일어납니다. 가격, 재고, 쿠폰을 한 트랜잭션 안에서 확인하기 때문에 주문이 동시에 몰려도 재고가 음수로 내려가지 않습니다.
- Postgres RLS 정책 53개와 트리거로 남의 주문을 보거나, 사지 않은 상품에 리뷰를 다는 걸 막았습니다.
- 장바구니, 주문, 결제, 마이페이지, 관리자 페이지까지 쇼핑몰에 필요한 화면을 모두 만들었습니다.

`Next.js` `TypeScript` `Supabase` `PostgreSQL` `Tailwind` `Vercel`

---

## 팀 프로젝트

### [경쟁사 모니터링 Agent](https://github.com/SkyWith628/competitor-monitoring-agent)
<sub>2026.11 · 4인 팀</sub>

> SPA 브랜드 네 곳의 상품, 가격, 뉴스, SNS를 모아서 매주 변화를 정리해 슬랙과 메일로 보내 주는 에이전트

넷이서 사이트 수집, 뉴스·SNS 수집, 분석, 플랫폼을 하나씩 맡았고, 저는 플랫폼과 PM을 맡았습니다. 넷이 동시에 작업해도 서로 코드를 덮어쓰지 않게 만드는 게 제 일이었습니다.

- 수집부터 발송까지 단계마다 주고받는 데이터 형식을 먼저 정해 두고, 각자 자기 폴더만 고치도록 나눴습니다.
- 저장소, 슬랙·메일 발송, CLI, 팀원용 대시보드(Next.js)를 만들었습니다.
- CODEOWNERS로 리뷰어를 자동 지정하고, main에는 PR, 담당자 승인, CI(ruff + pytest) 통과 없이는 합칠 수 없게 규칙을 걸었습니다. 매일 수집과 주간 리포트는 GitHub Actions로 돌아갑니다.

`Python` `GitHub Actions` `PostgreSQL` `Next.js`

---

## 그 밖에 만든 것들

| 프로젝트 | 시기 | 내용 | 링크 |
| --- | --- | --- | --- |
| Snip (URL 단축) | 2026.11 | FastAPI, MySQL, Nginx 컨테이너 3개를 AWS ECS Fargate에 올렸습니다. 동시에 20개를 만들어도 코드가 겹치지 않는지, 대소문자만 다른 코드가 같은 걸로 잡히지 않는지 확인했습니다. | [코드](https://github.com/SkyWith628/url-shortener) |
| toss-llm-trader | 2026.08 | LLM에게 주식 매매를 맡기면 그냥 사 두는 것보다 나은지 모의투자로 재 보는 실험입니다. 백테스트가 미래 가격을 미리 보지 못하게 막는 데 신경을 많이 썼습니다. | [코드](https://github.com/SkyWith628/toss-llm-trader) |
| 케어노트AI | 2026.10 | 요양보호사가 말로 남긴 내용을 방문요양 기록지 초안으로 바꿔 주는 서비스 기획 (2026 지능정보 SW 아이디어 경진대회) | [코드](https://github.com/SkyWith628/carenote-ai) |
| OBLIGE | 2026.05 ~ 06 | 화장품 공병을 반납하면 포인트와 등급이 쌓이는 쇼핑몰 | [코드](https://github.com/SkyWith628/oblige) · [사이트](https://skywith628.github.io/oblige/) |
| JABIS | 2026.03 | 음성으로 대화하는 학습 도우미 데스크톱 앱 (Electron) | [코드](https://github.com/SkyWith628/jabis) |
| Todo List | 2026.01 | React와 TypeScript로 만든 할 일 관리 앱 | [코드](https://github.com/SkyWith628/todo-list) · [사이트](https://skywith628.github.io/todo-list/) |
| 환율과 코스피 | 2025.09 ~ 12 | 환율이 코스피에 주는 영향을 회귀, PCA, ARIMA로 분석했고 KIPS 학술대회에서 발표했습니다. | [코드](https://github.com/SkyWith628/kospi-exchange-analysis) |
| shop69 | 학부 수업 | 프레임워크 없이 PHP와 MySQL로 만든 첫 쇼핑몰입니다. 나중에 같은 주제를 CHECKMATE로 다시 만들었습니다. | [코드](https://github.com/SkyWith628/php_shop_project) |
| iOS Practice | 2025.09 ~ 12 | Swift, UIKit 실습 앱 모음 | [코드](https://github.com/SkyWith628/ios-practice) |
| Data Analysis | 2025.09 ~ 12 | pandas, numpy 실습 노트북 | [코드](https://github.com/SkyWith628/Data_Analysis_Python) |
| Unity Game Basics | 2024.09 ~ 12 | Unity, C# 입문 실습 | [코드](https://github.com/SkyWith628/unity-game-basics) |

---

## 해 본 것들

기술 이름만 적으면 잘 와닿지 않을 것 같아서, 실제로 해 본 일 위주로 적었습니다.

<table>
<tr>
<td width="50%" valign="top">

**백엔드**
- 오래 걸리는 작업을 큐로 빼고 요청에는 바로 응답하기 (CodeSage)
- 같은 요청이 두 번 들어와도 한 번만 처리하기, 동시에 수정할 때 버전을 확인해서 덮어쓰지 않기 (Persona_ize)
- 외부 API나 DB를 바꿔 끼울 수 있게 인터페이스로 분리하기 (직구곰)

</td>
<td width="50%" valign="top">

**데이터베이스**
- 주문 금액 계산을 DB 함수로 옮기고 RLS로 접근 막기 (CHECKMATE)
- UNIQUE 제약과 콜레이션 설정으로 중복 코드 막기 (Snip)
- pgvector와 Neo4j 검색 결과를 같은 파이프라인에서 쓰기 (Persona_ize)

</td>
</tr>
<tr>
<td width="50%" valign="top">

**LLM**
- LLM 출력을 규칙으로 검사하고, 실패하면 다시 만들거나 원문으로 돌아가기 (Persona_ize)
- LLM이 할 일과 코드가 할 일을 나누고, 위험한 건 사람 승인 받기 (직구곰)
- 조건을 바꿔 가며 반복 실험하고 결과를 숫자로 남기기 (Persona_ize, toss-llm-trader)

</td>
<td width="50%" valign="top">

**배포와 협업**
- AWS ECS에 컨테이너 올리기. 처음엔 태스크가 뜨지도 않았는데, 로그 그룹을 만들 권한이 없어서였습니다. (Snip)
- Docker Compose와 Caddy로 HTTPS 배포하고 백업·점검 스크립트 만들기 (Persona_ize)
- GitHub Actions로 CI와 정기 작업 돌리기, 브랜치 보호 규칙 걸기 (경쟁사 모니터링)

</td>
</tr>
</table>

이 밖에 PHP, Electron, Swift, Unity도 수업과 개인 작업에서 써 봤습니다.

---

## 학교 활동

- 2026 컴퓨터소프트웨어학과 학회장
- 38대 총학생회 기획부장
- iOS 전공 멘토
- 2024 글로벌인덕리더스쿨 팀장

정보처리기능사가 있고, 한국어와 영어를 씁니다. 쉬는 날엔 주로 달립니다.

<br>

<div align="center"><sub>읽어 주셔서 감사합니다.</sub></div>
