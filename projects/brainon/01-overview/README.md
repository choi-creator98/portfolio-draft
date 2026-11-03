[← BrainOn Overview](../README.md) · [Portfolio Home](../../../README.md) · [Original Team Repository](https://github.com/brainCDSS/BrainCDSS/tree/dev)

## 01. Project Overview & My Role

**BrainOn**은 뇌혈관질환 환자의 임상정보, 의료영상 AI 분석, 의약품 안전성 검증 및 AI Assistant를 하나의 진료 Workflow로 연결하는 임상의사결정지원시스템입니다. 분리된 기능과 데이터를 연계하고, 사용자 요청부터 데이터 처리·AI 분석·결과 확인까지 일관된 흐름으로 이어지는 서비스 구조를 구현하는 것을 목표로 했습니다. **요청 → 임상정보 처리 → AI 분석·안전성 검사 → 결과 확인**으로 이어지는 임상 Workflow를 지향했습니다.

### What I Owned

| Responsibility | My Contributions |
| :--- | :--- |
| **Backend & Clinical Workflow** | Django/DRF 기반 REST API·비즈니스 로직, Imaging/DICOM·Surgical Planning 연계 및 JWT/RBAC 공동 구현 |
| **Database & Data Integrity** | PostgreSQL 데이터 모델·관계 설계, 상태·트랜잭션·동시성 및 정합성 관리 |
| **Medication Safety** | 식약처 DUR OpenAPI, 기준정보 저장·동기화, Medication Safety·CDS Hooks 연계 |
| **AI Assistant & MCP** | Django Assistant Gateway, MCP Clinical Tool 및 Vertex AI Gemini 연계 경로 구성 |
| **AI Inference & Notifications** | FastAPI Inference 요청·결과 연계, Celery 작업 흐름 및 FCM Web/Mobile 알림 통합 |
| **Async Processing** | RabbitMQ/Celery Queue·Worker·Beat, Outbox 발행·정기 작업과 실패 처리 구성 |
| **Cloud Infrastructure & Deployment** | GCP VPC·Cloud Run·Cloud SQL·VM 환경 구성, 서비스 배포·운영 환경 통합 및 네트워크·DB·외부 API 연결 검증 수행 |
| **Observability & Traceability** | Grafana·Prometheus·Loki·Alloy, 운영 대시보드, `request_id`·AuditLog·오류 추적 체계 |
| **System Integration & E2E Validation** | FE·AI 담당자와 API 계약을 조율하고, 전체 서비스 연계·통합 테스트·E2E 검증·배포 후 점검·Git 병합 및 트러블슈팅 수행 |

> **My Role**  
> 3인 팀 프로젝트에서 Backend 개발과 클라우드 인프라 구성, 서비스 배포 및 통합·E2E 검증을 담당했습니다.

---

---

[BrainOn 목차](../README.md) · [02. 시스템 아키텍처 →](../02-architecture/README.md)
