[← Portfolio Home](../README.md) · [Featured Project](./featured-project.md) · [Experience](./professional-experience.md) · [Technical Competencies](./technical-competencies.md) · [Projects & Contributions](./projects-contributions.md)

---

## <img src="../assets/icon-projects.png" alt="" width="22" height="22"> Projects & Contributions

<img src="../assets/accent-projects.png" alt="" width="270" height="6">

### BrainOn CDSS

**AI 기반 뇌혈관질환 임상의사결정지원시스템** | **2026.08 – 2026.10** | **3인 팀 프로젝트**

**Repository:** [brainCDSS/BrainCDSS (`dev`)](https://github.com/brainCDSS/BrainCDSS/tree/dev)

진료 Workflow에 의료영상 AI 분석, 의약품 안전성 검사, AI Assistant를 연결한 프로젝트입니다. Django 기반 Backend와 데이터 연동 기능을 구현하고, 클라우드 환경 구성·배포 및 서비스 간 통합 검증을 담당했습니다.

#### 주요 구현 및 코드 기여

| 영역 | 구현 및 작업 내용 | 코드 |
| :--- | :--- | :--- |
| **Clinical Backend** | Imaging Order·Study·History·Viewer Session API와 Surgical Planning 기능 구현. 권한, 상태, 버전 관리 및 임상 데이터 간 정합성 검증 | [Imaging·Surgical Planning](https://github.com/brainCDSS/BrainCDSS/commit/4936efd42bf8e118b1e2c7bc3aeb70c9811a82e2) |
| **Medication Safety** | 식약처 DUR OpenAPI 연동, 기준정보 DB 저장·동기화, 처방 안전성 검사 및 CDS Hooks 연계 | [PR #35](https://github.com/brainCDSS/BrainCDSS/pull/35) |
| **Assistant & MCP** | Django Assistant API·Assistant Service·MCP Server 연결, 요청 Context 전달 및 Backend 조회 API 연동 | [Assistant·MCP](https://github.com/brainCDSS/BrainCDSS/commit/a5460a939bcd73243b0a35377b2ba46ecff86ee9) |
| **Cloud & Integration** | GCP 기반 서비스 환경 구성·배포, Cloud Run 추론 서비스와 FCM 알림 연계, Backend·Frontend·AI 서비스 통합 검증 | [PR #36](https://github.com/brainCDSS/BrainCDSS/pull/36) |
| **Observability** | Grafana 운영 대시보드 및 비동기 작업 지표 구성, 사용자 오류 표시와 운영 로그 추적 기능 연계 | [Monitoring·Error Tracing](https://github.com/brainCDSS/BrainCDSS/commit/c54a26ddf0d123b52ae427902684b332005059a6) |
| **Audit & Traceability** | request_id 기반 추적, Audit 조회·접근 처리, 오류 코드와 관리자 감사 화면 연결 | [Audit·Request Tracking](https://github.com/brainCDSS/BrainCDSS/commit/4a9e70ece4b55258cab8541f555f3173bc829f53) |

<details>
<summary><strong>구현 과정에서 다룬 주요 문제 · 4가지</strong></summary>

- **Surgical Planning 상태·버전 및 API Routing**  
  환자·진료·영상·분석 결과 간 참조 정합성을 확인하고, 확정·취소 상태의 변경 제한과 버전 기반 수정 제어를 적용. `dev` 병합 과정에서 실제 구현 API와 Generated/Fallback API의 라우팅 우선순위를 정리.  
  [관련 코드](https://github.com/brainCDSS/BrainCDSS/commit/4936efd42bf8e118b1e2c7bc3aeb70c9811a82e2) · [PR #35](https://github.com/brainCDSS/BrainCDSS/pull/35)

- **DUR 기준정보와 처방 검사 연계**  
  식약처 기준정보를 로컬 DB에 저장하고 약품·성분 매핑 및 안전성 검사 흐름과 연결. 투여기간주의는 허용기간 이내·초과 시나리오를 구분해 검증.  
  [PR #35](https://github.com/brainCDSS/BrainCDSS/pull/35)

- **Assistant·MCP 요청 Context 전달**  
  Django API, Assistant Service, MCP Server 사이의 요청 Context 전달과 Backend 조회 경로 정리.  
  [관련 코드](https://github.com/brainCDSS/BrainCDSS/commit/a5460a939bcd73243b0a35377b2ba46ecff86ee9)

- **사용자 오류 표시와 내부 추적**  
  화면에 표시할 오류 정보와 내부 `request_id`, Audit 조회 및 Grafana 대시보드 연계를 구분해 구현.  
  [Monitoring·Error Tracing](https://github.com/brainCDSS/BrainCDSS/commit/c54a26ddf0d123b52ae427902684b332005059a6) · [Audit·Request Tracking](https://github.com/brainCDSS/BrainCDSS/commit/4a9e70ece4b55258cab8541f555f3173bc829f53)

</details>

#### 통합 및 검증

| 작업 | 검증 결과 |
| :--- | :--- |
| [PR #35 — DUR·Medication Safety·Imaging·Surgical Planning](https://github.com/brainCDSS/BrainCDSS/pull/35) | 관련 회귀 테스트 **15건 PASS**, MEDM → Medication Safety E2E 확인, Surgical Planning 병합 충돌 해결 및 Backend 검사 통과 |
| [PR #36 — FCM·Assistant/MCP·Cloud Run 연계](https://github.com/brainCDSS/BrainCDSS/pull/36) | Django check, Frontend test/build, Docker Compose 구성 검증 완료 |

<details>
<summary><strong>주요 코드 위치 바로가기</strong></summary>

| 코드 경로 | 확인할 내용 |
| :--- | :--- |
| [`backend/apps/imaging/`](https://github.com/brainCDSS/BrainCDSS/tree/dev/backend/apps/imaging) | Imaging Order·Study·History·Viewer Session API |
| [`backend/apps/surgical_planning/`](https://github.com/brainCDSS/BrainCDSS/tree/dev/backend/apps/surgical_planning) | 상태·버전·권한 및 Planning 기능 |
| [`backend/apps/medication_safety/`](https://github.com/brainCDSS/BrainCDSS/tree/dev/backend/apps/medication_safety) | 처방 안전성 검사 및 DUR 연계 |
| [`assistant-service/src/`](https://github.com/brainCDSS/BrainCDSS/tree/dev/assistant-service/src) · [`mcp-server/src/`](https://github.com/brainCDSS/BrainCDSS/tree/dev/mcp-server/src) | Assistant 흐름과 MCP 도구 |
| [`infra/observability/grafana/dashboards/`](https://github.com/brainCDSS/BrainCDSS/tree/dev/infra/observability/grafana/dashboards) | 운영·API·비동기·Audit 대시보드 |
| [`backend/apps/audit/`](https://github.com/brainCDSS/BrainCDSS/tree/dev/backend/apps/audit) | Audit 조회·접근 처리 |

</details>

**[BrainOn 구현 과정·설계·트러블슈팅 자세히 보기](./featured-project.md)**

---

### Previous GitHub — Python & Data Practice

**GitHub:** [choi-creator07](https://github.com/choi-creator07)

| 저장소 | 내용 | 바로가기 |
| :--- | :--- | :--- |
| **Python Exercises** | Python 문법·문자열·정규표현식, NumPy·Pandas·Matplotlib, openpyxl, Selenium 실습 | [python_ex_2603](https://github.com/choi-creator07/python_ex_2603) |
| **Medical Data Mini Project** | NumPy·Pandas·Matplotlib를 사용한 의료 기초 데이터 분석 노트북 | [py_data_2603](https://github.com/choi-creator07/py_data_2603/blob/main/proj.ipynb) |

#### 기타 실습 코드

- [NumPy 배열·연산 실습](https://github.com/choi-creator07/python_ex_2603/tree/main/numpy)
- [Pandas 데이터 처리 실습](https://github.com/choi-creator07/python_ex_2603/tree/main/pandas)
- [Selenium 웹 자동화 실습](https://github.com/choi-creator07/python_ex_2603/tree/main/selenium)
- [의료 기초 데이터 분석 노트북](https://github.com/choi-creator07/py_data_2603/blob/main/proj.ipynb)

---

### 관련 링크

[BrainOn Repository](https://github.com/brainCDSS/BrainCDSS/tree/dev) · [BrainOn 프로젝트 상세](./featured-project.md) · [Previous GitHub](https://github.com/choi-creator07)

---

[← 메인 포트폴리오로 돌아가기](../README.md)
