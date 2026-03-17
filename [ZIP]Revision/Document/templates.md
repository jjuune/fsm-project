** lagels.md

.github 아래는 보통 “자동화/템플릿” 폴더라, 문서면 docs/90-appendix/labels.md 또는 docs/90-appendix/xx-labels.md가 더 자연스럽긴 해.
그래도 쭌이 .github/labels.md로 두고 싶으면 OK.

표가 깨져있어(헤더 중복, 열 정의 틀림) → 표 정리 필요

Status/Risk 섹션이 한 줄로 붙어서 가독성 떨어짐 → 마크다운 구조로 정리

지금은 Project 필드(Type/Gate/Area/Priority)도 쓰고 있으니, **“라벨 vs 필드의 역할 분리”**를 2줄만 박아두면 운영이 훨씬 편해짐







1) Epic 템플릿

경로: .github/ISSUE_TEMPLATE/epic.md

---
name: "Epic"
about: "상위 작업(여러 티켓을 묶는 큰 목표)"
title: "[EPIC][G?] <Epic title>"
labels: ["epic"]
assignees: []
---

## Goal
(이 Epic이 끝나면 무엇이 가능해지나요? 한 문장)

## Context
- 배경:
- 왜 지금 필요한가:

## Scope (In)
- [ ] 

## Scope (Out)
- [ ] 

## Exit Criteria (Done Definition)
- [ ] (Gate 통과 조건 / 데모 가능 조건)

## Milestone / Gate
- Gate: G? (G1/G2/G3/G4)
- Priority: P? (P0/P1/P2)

## Risks / Notes
- 리스크:
- 의존성:

## Tickets (fill issue numbers after creation)
- [ ] T?? - (#___)
- [ ] T?? - (#___)


2) Ticket 템플릿

경로: .github/ISSUE_TEMPLATE/ticket.md

---
name: "Ticket"
about: "개별 작업 단위(구현/수정/테스트/문서)"
title: "[T??][G?][Area] <Ticket title>"
labels: ["ticket"]
assignees: []
---

## Goal
(이 티켓이 끝나면 무엇이 가능해지나요? 한 문장)

## Scope
### In
- [ ] 

### Out
- [ ] 

## Acceptance Criteria
- [ ] (완료 판정 조건 3~7개)

## Dependencies
- Parent Epic: #___
- Related tickets: #___

## QA / Test
- [ ] (테스트 방법: 로컬/스테이징/케이스)
- [ ] (실패 케이스가 있으면 명시)

## Notes
- 정책/토글:
- 리스크:


3) Bug 템플릿

경로: .github/ISSUE_TEMPLATE/bug.md

---
name: "Bug"
about: "버그 리포트 (재현/기대/실제/로그 중심)"
title: "[BUG][G?] <short summary>"
labels: ["ticket", "qa"]
assignees: []
---

## Summary
(버그를 한 문장으로 요약)

## Environment
- App/Module: (Admin/TM/Tech, Web/Mobile Web, Backend API 등)
- Branch/Commit: 
- Browser/Device:
- URL/Endpoint:
- Account role: (ADMIN/TM/TECH)

## Steps to Reproduce
1.
2.
3.

## Expected Result
(기대 동작)

## Actual Result
(실제 동작)

## Impact
- Severity: (P0-blocker / P1-core / P2-nice 중 하나로 표기)
- Frequency: (항상/가끔/특정 조건)

## Evidence
- Screenshot/Video:
- Console log:
- Server log / Request ID:
- Payload (가능하면):

## Acceptance Criteria (Fix Done)
- [ ] 재현 케이스에서 버그가 더 이상 발생하지 않음
- [ ] 관련 회귀 테스트(또는 체크리스트) 추가/통과
- [ ] (필요 시) 문서/UX 카피 업데이트

## Notes
- 추정 원인/가설:
- 우회 방법(있다면):


