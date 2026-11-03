[← BrainOn Overview](../README.md) · [Portfolio Home](../../../README.md) · [Original Team Repository](https://github.com/brainCDSS/BrainCDSS/tree/dev)

## 06. Engineering Challenges & Troubleshooting

BrainOn 개발 과정에서는 Backend 기능 구현뿐 아니라 클라우드 네트워크 연결, 서비스 간 API 통합, AI Assistant의 접근 제어 및 운영 장애 추적과 관련된 문제를 해결해야 했습니다.

문제를 단편적으로 수정하기보다 **원인 분석 → 영향 범위 파악 → 구조적 개선 → 검증** 과정을 중심으로 접근했습니다. 다음 사례는 실제 충돌 해결, 설계 제약 개선, 운영 관측성 보강을 구분하여 정리했습니다.

| Case | Challenge / Engineering Response |
| :--- | :--- |
| **Cloud Connectivity** | VPC·Cloud Run·VM·Cloud SQL 통신 경로와 접근·Egress 조건 분석 |
| **Merge & API Routing** | 병렬 개발 충돌 해결, Generated API와 실제 구현 API 우선순위 정리 |
| **MCP Context** | 환자 미선택 질문 허용과 환자별 접근 제어 유지 |
| **Error Tracking** | 내부 UUID와 사용자용 Tracking Code 분리, 로그·감사 기록 연계 |
| **Partial Failures** | HTTP 성공 응답 속 의존성 오류를 별도 운영 신호로 분류 |

<details>
<summary><strong>Case 01 · Cloud / VPC 서비스 통신 분석</strong></summary>

**Challenge: 클라우드 환경에서 서비스 간 통신 경로 구성 및 연결 문제 분석**

**Problem**

- Backend, RabbitMQ/Celery, Cloud SQL 및 AI 서비스가 서로 다른 GCP 실행 환경에서 동작
- 비공개 네트워크 구성을 추진하면서 서비스 간 통신 경로와 접근 정책을 구분하여 점검해야 하는 문제 발생

**Investigation & Approach**

- Cloud Run의 VPC Egress 및 서비스 접근 설정 확인
- Compute Engine VM의 내부 IP, 서비스 포트 및 네트워크 연결 상태 점검
- Cloud SQL의 Public/Private IP 구성과 Private Service Access 연결 조건 검토
- 내부 통신과 외부 API 호출을 구분하여 네트워크 경로 및 인증 조건 확인
- Cloud NAT의 Outbound 조건과 VM·Cloud SQL 연결 경로를 서비스별로 나누어 점검

**Engineering Outcome**

- 서비스별 네트워크 의존성과 통신 경로를 구분하여 인프라 구성 및 연결 검증 진행
- 애플리케이션 오류와 네트워크·접근 정책 문제를 별도로 분석할 수 있도록 점검 범위 정리

**Key Learning:** Cloud 환경에서는 서비스가 실행 중인 상태와 다른 서비스에서 정상적으로 접근할 수 있는 상태를 구분해야 하며, 네트워크 경로·인증·접근 정책을 함께 확인하는 것이 중요합니다.

> VPC 내부 통신과 외부 API 연결은 실제 구성 시점에 따라 달라졌습니다. 최종 통신 검증 기록이 확인되지 않은 경로는 완전한 비공개 운영 또는 장애 해결 완료로 단정하지 않았습니다.

</details>

<details>
<summary><strong>Case 02 · Surgical Planning Merge Conflict & API Routing 해결</strong></summary>

**Challenge: 병렬 개발 과정의 Git 충돌과 API Routing 정합성 문제 해결**

**Problem**

- 팀원별 기능 개발 이후 최신 `dev` 브랜치를 통합하는 과정에서 Surgical Planning 관련 Backend 코드 충돌 발생
- 실제 구현한 API와 기존 Generated/Fallback API가 공존하여 Routing 우선순위 정리 필요

**Root Cause & Analysis**

- 병렬 개발로 동일한 Serializer, View, Service 및 URL 설정 파일이 변경됨
- 기존 자동 생성 API와 실제 구현 API 간의 호출 경로를 명확하게 구분해야 했음

**Resolution**

- 최신 `dev` 변경사항을 개인 작업 브랜치에 반영하고 충돌 파일별 구현 내용 비교
- 검증된 Surgical Planning Backend 구현을 기준으로 충돌 해결
- `config/urls.py`에서 실제 구현 API가 Generated/Fallback API보다 우선 적용되도록 Routing 정리
- 전체 변경사항의 정합성을 확인한 후 통합 PR 생성

**Validation**

- Django System Check 정상
- Migration 누락 검사 정상
- DUR·Medication Safety 관련 Regression Test 15개 통과 (Surgical Planning 테스트 15개라는 의미는 아님)
- Medication Safety E2E 및 Surgical Planning 변경사항 검증
- 통합 PR 병합 완료

**Evidence**
- [PR #35 — Backend Integration & Merge Conflict Resolution](https://github.com/brainCDSS/BrainCDSS/pull/35)
- [Surgical Planning Implementation](https://github.com/brainCDSS/BrainCDSS/commit/4936efd42bf8e118b1e2c7bc3aeb70c9811a82e2)

**Key Learning:** Git 충돌 해결은 단순히 변경 내용을 병합하는 작업이 아니라, 기능 계약과 실제 API 동작을 유지하면서 팀 전체 코드를 통합하는 검증 과정입니다.

</details>

<details>
<summary><strong>Case 03 · MCP Patient / Global Context 제약 개선</strong></summary>

**Challenge: 환자 미선택 질문과 환자별 임상정보 조회의 처리 조건 충돌**

**Problem**

- 초기 Assistant 요청 구조에서는 `patient_id`와 환자 접근 범위를 요구
- 특정 환자와 무관한 일반 질문이나 전체 업무 현황 조회에도 환자 Context가 필요해지는 제약 발생
- 환자 미선택 질문을 허용하더라도 기존 임상정보 접근 통제는 유지해야 했음

**Root Cause & Analysis**

- Patient Tool과 Global Tool의 입력 조건이 구분되지 않은 요청 구조
- 환자별 조회에 필요한 접근 검증을 모든 질문에 동일하게 적용한 것이 문제

**Resolution**

- Patient Tool과 Global Tool을 구분하여 요청 Context 구조 개선
- 환자 미선택 일반 질문에서는 `patient_id` 없이 처리할 수 있도록 변경
- 질문에 필요한 MCP Tool만 선택적으로 호출하도록 로직 구성
- Patient Tool 사용 시 환자 식별자와 `patient_scope` 검증 유지
- MCP Registry에서도 허용된 Tool 및 접근 범위를 재검증
- Patient Tool에는 환자 ID가 필요하고 Global Tool에는 역할 검증이 필요하다는 조건을 분리

**Verification**

- 환자 Context가 필요한 질문과 필요하지 않은 질문의 처리 흐름 분리
- 일반 질문에서 불필요한 MCP 호출을 방지하는 로직 구현
- Patient Tool의 접근 범위 검증을 유지하고 관련 테스트 코드 추가

**Evidence**
- [MCP Routing & Request Tracking — Commit](https://github.com/brainCDSS/BrainCDSS/commit/a5460a939bcd73243b0a35377b2ba46ecff86ee9)
- [Assistant Chat Flow](https://github.com/brainCDSS/BrainCDSS/blob/dev/assistant-service/src/flows/chat.ts)
- [MCP Tool Registry](https://github.com/brainCDSS/BrainCDSS/blob/dev/mcp-server/src/tools/registry.py)

**Key Learning:** AI 서비스에서는 기능 확장뿐 아니라 요청의 맥락과 데이터 접근 범위를 분리하여, 사용성과 보안 조건을 동시에 유지하는 설계가 중요합니다.

</details>

<details>
<summary><strong>Case 04 · User-facing Error Code와 내부 Audit Trace 분리</strong></summary>

**Challenge: 사용자 오류 표시와 내부 요청 추적 식별자 분리**

**Problem**

- UUID 기반 `request_id`는 내부 요청 추적에 적합하지만 사용자에게 그대로 표시하기에는 가독성이 낮음
- HTTP 요청, 오류 응답, AuditLog 및 비동기 작업 간 연관 관계를 유지해야 함

**Analysis & Resolution**

- 내부 추적을 위한 `request_id`를 기준 식별자로 유지
- 사용자에게 표시할 별도의 `ERR-` 형식 Tracking Code 구성 (내부 `request_id`는 기준 식별자로 유지)
- 오류 응답 및 HTTP Header에 추적 정보를 연결
- 구조화 요청 로그에 서비스·Route·응답 상태·처리시간 및 요청 식별자 기록
- AuditLog 조회 및 운영 화면에서 요청 식별자를 활용할 수 있도록 연계

**Engineering Outcome**

- 사용자용 오류 코드와 시스템 내부 추적 식별자의 역할 분리
- 운영자가 오류 문의와 관련 요청·감사 정보를 연결하여 확인할 수 있는 코드 구조 구현
- 원본 UUID를 기준으로 서비스 요청의 연관 관계 유지

**Evidence**
- [Request Tracking Middleware](https://github.com/brainCDSS/BrainCDSS/blob/dev/backend/apps/common/middleware.py)
- [Tracking Code Implementation](https://github.com/brainCDSS/BrainCDSS/blob/dev/backend/apps/common/tracking.py)
- [Audit Request Tracking — Commit](https://github.com/brainCDSS/BrainCDSS/commit/4a9e70ece4b55258cab8541f555f3173bc829f53)

**Key Learning:** 장애 대응을 고려한 시스템에서는 로그를 남기는 것뿐 아니라, 사용자가 전달한 오류 정보로 관련 요청과 처리 이력을 찾을 수 있는 구조가 필요합니다.

</details>

<details>
<summary><strong>Case 05 · HTTP 2xx에 가려진 하위 서비스 오류 관측</strong></summary>

**Challenge: HTTP 성공 응답만으로 확인하기 어려운 내부 의존성 오류**

**Problem**

- HTTP 응답이 `2xx`여도 응답 내부의 외부 연동 또는 하위 서비스 처리 결과는 실패·지연·성능 저하 상태일 수 있음
- 단순 HTTP 상태 코드 모니터링만으로는 이러한 부분 실패를 식별하기 어려움

**Engineering Approach**

- 내부 의존성의 응답 상태를 별도로 판단하는 관측성 로직 구성
- `FAILED`, `TIMEOUT`, `DEGRADED`, `PARTIAL_FAILURE` 등 상태를 운영 신호로 분류
- Prometheus Counter, 구조화 Warning Log 및 `DEPENDENCY_DEGRADED` Audit 이벤트로 관련 상황을 기록하도록 구현
- API·비동기·인프라·감사 추적 대시보드를 구분하여 운영 지표 시각화

**Engineering Outcome**

- HTTP 응답 성공 여부와 내부 의존성 상태를 별도로 관측할 수 있는 코드 구조 마련
- 서비스 장애 및 부분 실패를 분석할 수 있도록 Metrics·Logs·Audit 정보 연계

**Evidence**
- [Dependency Observability Implementation](https://github.com/brainCDSS/BrainCDSS/blob/dev/backend/apps/common/dependency_observability.py)
- [Operations Monitoring & Error Tracing — Commit](https://github.com/brainCDSS/BrainCDSS/commit/c54a26ddf0d123b52ae427902684b332005059a6)

**Key Learning:** 서비스의 정상 동작 여부는 HTTP 응답 코드 하나로 판단하기 어렵습니다. 내부 의존성·작업 상태·로그를 함께 관측할 수 있어야 장애 원인을 보다 정확하게 분석할 수 있습니다.

</details>

### Engineering Takeaways

프로젝트를 통해 기능 구현과 시스템 운영을 별개의 작업으로 보지 않고, 설계 단계부터 데이터 흐름·통신 경로·실패 상황·추적 가능성을 함께 고려하는 경험을 쌓았습니다.

특히 Backend 기능을 구현하는 과정에서 Cloud Infrastructure, 비동기 작업, AI 서비스 연계 및 운영 모니터링까지 연결하여 **개발과 운영을 함께 이해하는 엔지니어링 관점**을 발전시켰습니다.

---

---

[← 05. Cloud 인프라 및 관측성](../05-cloud-observability/README.md) · [BrainOn 목차](../README.md) · [07. 검증 및 코드 기여 →](../07-validation/README.md)
