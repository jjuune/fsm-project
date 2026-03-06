# A-01 System Architecture Overview

## TL;DR
- Phase 1에서는 **Field Service Management(FSM)** 핵심 기능 구현에 집중
- Mobile Technician 중심의 가벼운 작업 처리 구조 지향
- REST API 기반 클라이언트-서버 구조
- 파일 업로드 / PDF 생성 / 알림 발송은 시스템 안정성을 위해 **비동기 배후 작업(Async Processing)**으로 처리

---

# 1. System Overview

본 시스템은 **현장 작업 관리(Field Service Management)**를 위한 웹 기반 플랫폼입니다.

### 핵심 목표:
- 작업 생성 및 배정 자동화
- 현장 기사 모바일 작업 처리 편의성 증대
- 체크리스트 및 서명 기록 기반의 디지털 증빙
- 완료 보고서(PDF) 자동 생성 및 고객 즉시 발송

---

# 2. Architecture Layers

## Client Layer (Frontend)
- **Role-based Web UI**: 역할별 전용 인터페이스 제공
  - **Admin Dashboard**: 본사 전체 운영 및 마스터 관리
  - **Team Manager Dashboard**: 팀별 작업 배정 및 기사 관리
  - **Technician Mobile Web**: 현장 특화 모바일 작업 UI

## Application Layer (Backend)
- **REST API Server**: 업무 로직 처리 및 데이터 인터페이스 제공
- **Core Services**:
  - `WorkOrder Service`: 상태 전이 및 배정 로직
  - `Checklist Service`: 템플릿 기반 항목 관리
  - `File Service`: 이미지 및 서명 파일 관리
  - `Notification Service`: SMS/Email 발송 연동
- **PDF Generation Worker**: 작업 완료 시 구동되는 비동기 작업기

## Data Layer (Database)
- **Primary Database**: PostgreSQL (또는 호환 RDBMS)
- **Core Entities**:
  - User / Team / Organization
  - Customer / Site
  - WorkOrder / Checklist / Attachment / Signature
  - AuditLog (Phase 1은 기초 스키마 설계 우선)

## Infrastructure Layer
- **File Storage**: AWS S3 또는 로컬 스토리지 (확장성 확보)
- **Message Queue**: 비동기 처리를 위한 큐 (Redis 등)
- **Background Worker**: 배후 작업 실행 엔진

---

# 3. Core Data Flow

### 작업 수행 흐름 (Assignment Chain)
1. **Admin**: WorkOrder 생성 및 팀 배정 (`DRAFT` → `TEAM_ASSIGNED`)
2. **Team Manager**: 담당 기사 배정 (`TEAM_ASSIGNED` → `TECH_ASSIGNED`)
3. **Technician**: 작업 수락/시작 및 증빙 수행 (`IN_PROGRESS`)
4. **증빙 입력**: 체크리스트 → 서명 → 사진 순차 입력
5. **완료 제출**: 상태 전이 (`COMPLETED`)

### 작업 완료 후 자동화 (Post-Processing)
1. **Status Trigger**: `COMPLETED` 상태 전이 시 트리거 실행
2. **Async Task**: 비동기 큐에 PDF 생성 작업 삽입
3. **Output**: 보고서 생성 완료 후 고객 대상 SMS/Email 발송

---

# 4. Security Model

### RBAC (Role-Based Access Control)
- **Admin**: ORG 범위 전체 데이터 조회/관리
- **Team Manager**: 자신이 속한 TEAM 범위 데이터 관리
- **Technician**: 자신에게 할당된 SELF 작업만 접근

> 상세 정책은 `20-product/00-rbac.md` 문서를 참조합니다.

---

# 5. Async Processing (비동기 처리)

시스템 가용성과 사용자 경험을 위해 다음 작업은 API 통신과 분리하여 **Background Worker**에서 비동기로 처리합니다.
- **PDF 생성 및 보관**
- **SMS 알림 발송**
- **Email 보고서 발송**

---

# 6. Phase Roadmap

### Phase 1: FSM Core
- 핵심 WorkOrder 프로세스 및 3단 배정 기반 구축
- 체크리스트, 서명, 사진 등 기초 증빙 디지털화

### Phase 1.5: 운영 안정화
- 알림 시스템 고도화 및 재시도 큐(Retry Queue) 도입
- Audit Log 감사 기능 및 운영 통계 강화

### Phase 2: Business 확장
- 제품 카탈로그 심화 연동
- 재고(Inventory) 관리 및 자재 수급 연동

### Phase 3: Platformization
- 멀티 테넌시(Multi-tenancy) 지원
- 구독 모델 및 조직별 맞춤형 RBAC 고도화
