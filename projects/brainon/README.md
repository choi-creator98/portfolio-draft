<p align="center">
  <a href="../../README.md">Portfolio Home</a> ·
  <a href="https://github.com/brainCDSS/BrainCDSS/tree/dev">Original Team Repository</a>
</p>

<p align="center">
  <img src="../../assets/hero-gradient.png" alt="BrainOn portfolio header" width="100%" />
</p>

<h1 align="center">BrainOn</h1>

<p align="center"><strong>Backend Engineering · Cloud Infrastructure · AI Service Integration · Operations</strong></p>

<p align="center">AI 기반 뇌혈관질환 임상의사결정지원시스템(CDSS) · 3인 팀 프로젝트 · 2026.08–10 (8주)</p>

---

> **Engineering Perspective**  
> 임상 업무와 AI 기능을 하나의 서비스로 연결하면서, 데이터 정합성·비동기 처리·접근 통제·운영 관측성을 함께 고려했습니다.  
> Backend 구현에서 Cloud Infrastructure 운영까지 이어지는 시스템 관점을 중심으로 기여했습니다.

## Project at a Glance

임상정보, 의료영상 AI 분석, DUR 기반 의약품 안전성 검증, AI Assistant를 하나의 진료 Workflow로 연결하는 프로젝트입니다.

| Focus | My Work |
| :--- | :--- |
| **Backend & Data** | Django/DRF 임상 API, PostgreSQL 데이터 모델·정합성, DUR·CDS Hooks |
| **AI & Asynchronous Integration** | MCP/Assistant·Inference 연계, RabbitMQ/Celery·Outbox·알림 |
| **Cloud & Operations** | GCP 인프라 구성, Cloud SQL·서비스 배포·통합 및 E2E 검증 |
| **Observability** | Grafana·Prometheus·Loki·Alloy, AuditLog·`request_id` 추적 |

> **My Role**  
> 3인 팀 프로젝트에서 Backend 개발과 클라우드 인프라 구성, 서비스 배포 및 통합·E2E 검증을 담당했습니다.

---

## Explore the Project

관심 있는 항목을 클릭하면 **별도의 README 페이지로 이동**합니다. 각 페이지에서는 핵심 설명을 먼저 확인하고, 상세 구현·트러블슈팅·코드 증빙을 토글로 펼쳐볼 수 있습니다.

| Section | What You'll Find |
| :--- | :--- |
| **[01. 프로젝트 개요 및 개인 담당 범위](./01-overview/README.md)** | Backend·DB·AI 연계·배포·E2E 검증의 담당 업무 |
| **[02. 시스템 아키텍처](./02-architecture/README.md)** | Django 중심 서비스 연결·설계 의사결정·원본 문서 |
| **[03. Backend 및 AI 서비스 통합](./03-backend-ai/README.md)** | 임상 API·MCP·AI 추론·DUR/CDS Hooks 구현 |
| **[04. 데이터 정합성 및 비동기 처리](./04-data-async/README.md)** | PostgreSQL·RabbitMQ/Celery·Outbox·재시도 |
| **[05. Cloud 인프라 및 관측성](./05-cloud-observability/README.md)** | GCP·Cloud SQL·배포·Grafana·로그·Audit |
| **[06. 트러블슈팅](./06-troubleshooting/README.md)** | VPC·API Routing·MCP·오류 추적·부분 실패 분석 |
| **[07. 검증 및 코드 기여](./07-validation/README.md)** | PR #35/#36·통합 테스트·직접 작성 Commit |

---

## Implementation Evidence

| Evidence | Source |
| :--- | :--- |
| **Clinical Backend / DUR / Imaging** | [Merged PR #35](https://github.com/brainCDSS/BrainCDSS/pull/35) |
| **Assistant / MCP / Inference Integration** | [Merged PR #36](https://github.com/brainCDSS/BrainCDSS/pull/36) |
| **Operations Monitoring & Grafana** | [Implementation Commit](https://github.com/brainCDSS/BrainCDSS/commit/c54a26ddf0d123b52ae427902684b332005059a6) |
| **Request Tracking & Audit** | [Implementation Commit](https://github.com/brainCDSS/BrainCDSS/commit/4a9e70ece4b55258cab8541f555f3173bc829f53) |

<p align="center"><a href="../../README.md">← Back to Main Portfolio</a></p>
