## 🚀 개요 (What & Why)

이 MR이 무엇을 변경하고자 하며, 왜 필요한지를 간략히 설명해주세요

<!-- proposal.md의 Why/What Changes 기반으로 작성 -->

## 📎 참고자료 (관련 이슈, 문서, 쓰레드)

연관된 WiKi 문서, 쓰레드 등을 링크해주세요.

<!-- JIRA 티켓 URL · Wiki · 쓰레드 등 관련 링크만 적는다. 한 줄 설명·요약을 덧붙이지 않는다. 없으면 "없음". -->

## 📌 주요 변경사항 (Changes)

실제 코드 변경의 핵심 포인트를 나열해주세요. (변경사항의 의미 단위로 작성)
리팩토링 사항이 있다면 함께 기재해주세요.

<!-- design.md + 델타 스펙 + git diff 요약 기반으로 작성.
     "무엇이 바뀌었는가"만 적는다 — 도메인/API 계약/이벤트 등.
     구현 디테일(필드 단위 NotNull, 메시지 문자열, 분기 알고리즘)은 코드/TC가
     이미 보여주므로 본문에 적지 않는다. "검증" 소제목 자체를 만들지 않는다. -->

## 📚 논의하고 싶은 사항, 리뷰 시 참고해야 할 사항이 있나요?

<!-- design.md의 Open Decisions, proposal.md의 제외 범위. 없으면 "없음".
     Open Decisions가 있으면 Q/A 형식을 유지한다:
     - Q: 질문
     - A: 현재 결정 + 근거 -->

## 🧪 테스트 및 방법 (How to Test)

QA 또는 리뷰어가 어떻게 기능을 확인할 수 있는지 구체적으로 작성해주세요.

<!-- "리뷰어가 직접 돌려볼 수 있는 방법"을 기준으로 작성한다.
     1) slophyspec/ 하위 TC 문서(frontmatter trigger.type이 있는 .md — slophyspec/changes/·slophyspec/templates/ 제외)가 있으면 케이스를 표(TC | 시나리오 | 기대)로 본문에 옮긴다.
        TC 문서 경로 링크는 적지 않는다 — 사내 리포 링크는 외부에서 렌더되지 않는다.
        테스트 클래스명만 나열하는 것은 충분하지 않다 — 리뷰어가 클래스를 따라가지 않는다.
     2) cURL/UI 시나리오를 함께 적되, 환경 의존 값은 플레이스홀더로 둔다:
        - 인증: `-u <user>:<password>` (실제 채널/패스워드 X)
        - 호스트: `<host>`
        - 헤더 식별자: `<modify-id>` 등
     3) "컴파일 성공", "단위테스트 통과"는 기재하지 않는다. -->

## 🧩 배포 전 확인사항

배포 전 챙겨져야 하는 체크리스트입니다. 확인하시고 체크 해주세요.

- [ ] PM 싱크 완료.
- [ ] 프론트 작업인가요? 시연 및 UI 리뷰를 진행해주세요.
- [ ] 취약점 점검이 필요한 작업인가요? 여부에 따라 요청해주세요.

<!-- AI가 채우지 않음 — 작성자가 직접 체크 -->

## 배포 필요 모듈

<!-- 다음 CLI 출력을 그대로 체크에 반영한다:
       slophyspec detect-deploy --target-branch <target>
     반환된 JSON `deploy_modules` 배열에 포함된 모듈만 [x], 나머지는 [ ].
     grep/추측 금지. CLI 실패 시 수동 확인 요청. -->

### 내부 API

- [ ] example-api
- [ ] example-internal-api

### 외부 API

- [ ] example-external-api

### 어드민

- [ ] example-admin

### 워커

- [ ] example-worker

### 배치 이미지

- [ ] example-batch
