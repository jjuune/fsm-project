# FSM Project Phase Roadmap

## TL;DR

이 문서는 FSM(Field Service Management) 시스템의 **Phase별 개발 범위를 한눈에 보기 위해 만든 로드맵**이다.

* **Phase 1 (MVP)**
  현장 기사들이 **설치 / AS 작업을 모바일에서 수행할 수 있는 최소 운영 시스템** 구축

* **Phase 2**
  **제품 / 설치 자산 / 보증 관리** 등 서비스 자산 관리 기능 추가

* **Phase 3**
  **스케줄링, 재고, 정산, 분석** 등 운영 및 비즈니스 기능 확장

이 문서는 **기능 설계 문서가 아니라 개발 범위(scope)를 정리하는 지도(map)** 역할을 한다.

---

# Phase 1 — MVP (Field Operation Core)

## 목표

현장 기사들이 **설치(Installation) 또는 AS(Repair) 작업을 모바일 환경에서 수행하고 기록할 수 있는 최소 운영 시스템을 구축한다.**

이 단계에서는 실제 현장 업무 흐름을 디지털화하는 것에 집중한다.

## 핵심 기능

* 사용자 인증 (Authentication)

* 권한 관리 (RBAC)

* 팀 관리

* 사용자(기사) 관리

* 고객(Customer) 관리

* 설치 위치(Site) 관리

* 작업지시서(Work Order) 생성

* 작업지시서 배정 (기사 할당)

* 작업 상태 관리 (FSM)

* 체크리스트 수행

* 사진 첨부

* 서명(Signature) 수집

* 작업 결과 PDF 리포트 생성

* 기본 대시보드

## 지원 WorkOrder 타입

Phase 1에서는 두 가지 작업 유형만 지원한다.

* **INSTALLATION** (설치)
* **REPAIR** (AS / 수리)

두 작업 유형은 기본 구조는 동일하며 다음 항목에서만 차이가 있다.

* 체크리스트 템플릿
* 작업 리포트 양식

## 핵심 도메인 엔티티

Phase 1에서 사용되는 주요 데이터 엔티티는 다음과 같다.

* Organization

* Team

* User

* Customer

* Site

* WorkOrder

* WorkOrderAssignee

* ChecklistTemplate

* ChecklistItem

* Attachment (사진)

* Signature

---

# Phase 2 — Product & Asset Layer

## 목표

설치된 제품과 서비스 자산을 관리할 수 있도록 시스템을 확장한다.

Phase 1에서 생성된 WorkOrder 데이터를 기반으로 **설치 자산과 서비스 이력 관리** 기능을 추가한다.

## 주요 기능

* 제품 카탈로그 관리

* 제품 카테고리 관리

* 설치된 제품(Installed Asset) 관리

* 자산 기반 서비스 이력 관리

* 보증(Warranty) 관리

* 자산 기반 WorkOrder 생성

---

# Phase 3 — Operations & Business Layer

## 목표

현장 운영을 최적화하고 비즈니스 운영 기능을 확장한다.

이 단계에서는 FSM 시스템을 **운영 플랫폼 수준으로 확장**하는 것을 목표로 한다.

## 주요 기능

* 작업 스케줄링 최적화

* 기사 작업 배정 자동화

* 재고 관리

* 부품 관리

* 청구 / 정산 / 인보이스

* 운영 분석 대시보드

* SLA 모니터링

---

# Notes

이 문서는 다음 목적을 위해 사용된다.

* Phase별 **개발 범위 정의**
* ERD 설계 범위 기준
* API 설계 범위 기준
* 기능 우선순위 판단 기준

새로운 기능을 추가할 때는 항상 먼저 다음 질문을 한다.

> 이 기능은 Phase 1 / Phase 2 / Phase 3 중 어디에 속하는가?
