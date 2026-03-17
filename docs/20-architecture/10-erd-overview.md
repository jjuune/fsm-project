# 01-erd-overview (Database Architecture)

## TL;DR
- **구조:** WorkOrder 중심의 1:N 정규화 (Checklist, History, Signature 분리)
- **무결성:** Snapshot(데이터 고정) + History(action_type 기반 추적) + Audit(전 테이블 created/updated 필드)
- **제약:** 1:N 서명(Enum 기반), 사유 Enum화, WorkOrder별 중복 템플릿 방지(Unique Index)

---

## 1. Overview (개요)

본 프로젝트의 데이터베이스는 **WorkOrder를 중심**으로 모든 비즈니스 활동이 기록되는 구조를 가집니다.  
과거 데이터의 무결성을 보장하기 위해 **Snapshot + History 기반 설계**를 적용하며, 모든 테이블에 생성/수정 시각을 기록하여 운영의 투명성을 확보합니다.

---

## 2. Core Entities (핵심 테이블 정의)

### 📦 2.1 WorkOrder
*   `incomplete_reason_type` / `cancel_reason_type`: **Enum** (운영 KPI 분석용)
*   `created_at` / `updated_at`: 전 생애주기 추적

### 👥 2.2 Organization (Teams & Technicians)
*   모든 테이블에 `status` (ACTIVE/INACTIVE) 및 `updated_at` 포함.

### ✅ 2.3 Checklist
#### workorder_checklists
*   **Constraint:** `(work_order_id, template_id)`에 **Unique Index** 적용.
*   **이유:** 동일 작업에 동일한 점검 템플릿이 중복 할당되는 비즈니스 오류 원천 차단.

#### workorder_checklist_items
*   `checked` (Bool) & `value` (Text) 병행 구조.
*   **Rule:** `input_type`이 BOOLEAN이면 `checked`만 사용, TEXT/NUMBER면 `value`만 사용하도록 서비스 레이어에서 강제함.

---

### 📸 2.4 Proof
#### signatures
*   `sign_type`: **Enum (CUSTOMER / TECHNICIAN)** - 문자열 오타 방지 및 다중 서명 필터링 최적화.

#### photos
*   `photo_type`: **Enum (PRE / POST / ISSUE)** - 현장 이슈 증빙 전용 타입 포함.

---

### 🧾 2.5 History
#### workorder_histories
*   `action_type` (STATUS_CHANGE / UPDATE_FIELD / ADMIN_OVERRIDE)
*   `field_name`, `old_value`, `new_value`: Admin 오버라이드 또는 정보 수정 시 변경 전/후 값 보존.

---

## 3. Relationship Map (ER Diagram)
*(Mermaid 다이어그램 생략 - DBML 참조)*

---

## 4. Key Design Decisions (최종)
1.  **Strong Audit Trail:** 모든 테이블에 `created_at`, `updated_at`을 기본 탑재하여 데이터 관리 투명성 확보.
2.  **Snapshot Strategy:** 외부 마스터 데이터 변경과 무관하게 과거 작업 보고서의 신뢰성 유지.
3.  **Strict Constraints:** Unique Index와 Enum을 활용하여 DB 레벨에서의 데이터 오염 방지.

---

## 7. Notes (운영 정책 핵심)

### 🔄 WorkOrder 상태 롤백 (Rollback) 정책
*   **원칙:** `COMPLETED` 또는 `CANCELED` 상태의 작업은 일반 기사/팀장이 다시 되돌릴 수 없음.
*   **예외:** 데이터 오입력 등 정당한 사유가 있는 경우, **Admin 권한에 한해** `ASSIGNED` 또는 `IN_PROGRESS`로 롤백 가능.
*   **강제 조건:** 롤백 수행 시 반드시 `workorder_histories`에 `ADMIN_OVERRIDE` 액션 타입과 상세 사유를 남겨야 함.

### ✅ 완료 조건 검증 (Service Logic)
*   DB는 데이터의 그릇이며, **완료 조건(필수 체크리스트, 서명 존재 등)은 반드시 백엔드 서비스 레이어에서 최종 검증**한 후 상태를 변경함.
