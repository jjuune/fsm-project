# FSM (Field Service Management) Project

## TL;DR
- **목표:** 공간살균기 설치·AS 운영의 100% 디지털 전환 및 표준화
- **핵심:** WorkOrder 중심의 상태 관리(FSM)와 데이터 무결성(Snapshot/History) 확보
- **스택:** React (Front) + Node.js (Back) + PostgreSQL (DB)

## 🧠 Core Concept

FSM 시스템은 **WorkOrder 중심 시스템**입니다.
모든 데이터와 기능은 WorkOrder의 생성, 배정, 수행, 완료 과정에 종속됩니다.

### Core Flow
Admin → WorkOrder 생성  
→ Team 할당  
→ Technician 배정  
→ 작업 수행  
→ 증빙 수집 (Checklist / Signature / Photo)  
→ 완료 처리

### Supporting Domains
- **Team / Technician:** 작업 수행 주체
- **Checklist:** 작업 검증 기준
- **Product:** WorkOrder 생성 시 참조 정보

---

## 🛠 Tech Stack (Proposed)

본 프로젝트는 확장성과 유지보수성을 고려하여 아래의 기술 스택을 권장합니다.

*   **Frontend:** React.js (TypeScript) / Tailwind CSS (UI)
*   **Backend:** Node.js (Express or NestJS)
*   **Database:** PostgreSQL (Relational Data + JSONB for flexibility)
*   **Storage:** AWS S3 (Signatures, Photos, PDF Reports)
*   **Infrastructure:** Docker (Containerization)

---

## 📂 Project Structure

```text
.
├── src/                # (Pending) Source Code
│   ├── frontend/       # React Frontend
│   └── backend/        # Node.js API Backend
├── docs/               # System & Business Documentation
│   ├── 10-architecture/ # Core Logic & DB Schema (FSM, ERD)
│   ├── 20-interface/    # API Contracts & UI Mapping
│   ├── 30-domain/       # Business Rules & Policies
│   └── _archive/        # Legacy Drafts & History
└── README.md           # This file (Engineering Hub)
```

---

## 📜 Engineering Principles (Hard Rules)

시스템의 무결성을 위해 모든 개발자는 아래 원칙을 반드시 준수해야 합니다.

1.  **Data Preservation:** 어떠한 경우에도 데이터를 물리적으로 삭제(`DELETE`)하지 않습니다. 대신 `status='INACTIVE'` 또는 `deleted_at` 컬럼을 활용한 **Soft Delete**를 수행합니다.
2.  **State Integrity:** 모든 워크오더의 상태 변경은 정의된 **FSM(Finite State Machine)** 규칙을 따라야 하며, 전이 시 반드시 로그(History)를 남깁니다.
3.  **API Standard:** 모든 API 응답은 일관된 JSON 포맷을 유지해야 하며, 인증된 Scope(ORG/TEAM/SELF)에 따른 엄격한 데이터 필터링을 적용합니다.
4.  **Proof Completion:** 완료(`COMPLETED`) 처리는 필수 체크리스트 응답, 서명 데이터, (선택 시) 사진 증빙이 서버 사이드에서 검증된 후에만 확정됩니다.

---

## 🧭 Documentation Links

상세한 설계 및 요구사항은 아래 문서들을 참조하십시오.

*   **[Core Architecture]**
    *   [WorkOrder FSM (State Model)](docs/10-architecture/01-workorder-fsm.md)
    *   [Database Schema & ERD](docs/10-architecture/02-database-schema.md)
*   **[Interface & Specs]**
    *   [API Contract & Security](docs/20-interface/01-api-contract.md)
    *   [UI Component & Routing Map](docs/20-interface/02-frontend-routing.md)
*   **[Business & History]**
    *   [Phase 1 SSoT (Single Source of Truth)](docs/30-domain/00-phase1-functional-spec.md)
    *   [Archive: Legacy Business Drafts](docs/_archive/v0_business_drafts/README_legacy.md)

---

## 🚀 Getting Started

*(개발 환경 구축 및 실행 방법은 초기 코드 베이스 구축 시 업데이트 예정입니다.)*
