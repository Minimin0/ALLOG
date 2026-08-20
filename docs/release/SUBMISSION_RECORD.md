# ALLOG Submission Record

> 이 문서는 실제 제출 직전 **제출본을 재현할 수 있는 최소 기록**을 남기는 곳입니다.
>
> 9단계 Production 통합 테스트가 끝나기 전에는 Final 필드를 임의로 채우지 않습니다.

## Submission status

```text
STATUS: NOT_FROZEN
```

허용 상태:

- `NOT_FROZEN`
- `FROZEN_FOR_SUBMISSION`
- `SUBMITTED`
- `BLOCKED`

---

## Current release candidates

작성 시점 기준 현재 `main` 후보입니다.

| Component | Repository | Candidate SHA | Final SHA |
| --- | --- | --- | --- |
| Frontend | `Minimin0/ALLOG-Frontend` | `6c0c747cfbe0d07afa057f6320b4a3539771123f` | `TBD` |
| Backend | `Minimin0/ALLOG-Backend` | `0e27e033acb45a6e83c49b9f139fb352ddf51ade` | `TBD` |

> Candidate SHA는 제출 전에 바뀔 수 있습니다. Final SHA는 최종 통합 테스트가 PASS한 정확한 `main` SHA만 기록합니다.

---

# Frontend submission record

```text
Repository: Minimin0/ALLOG-Frontend
Branch: main
Final commit SHA: TBD
Tag: hackathon-final-2026

Production API: https://api.allog-app.store
Artifact filename: TBD
Artifact type: TBD
Artifact SHA-256: TBD
Build timestamp: TBD
Build environment: TBD
```

### Frontend final gate

- [ ] `git rev-parse HEAD` recorded
- [ ] working tree clean
- [ ] `npm ci` PASS
- [ ] API check PASS
- [ ] Auth store check PASS
- [ ] Onboarding check PASS
- [ ] Font check PASS
- [ ] Canonical UI runtime check PASS
- [ ] Video/JPEG boundary check PASS
- [ ] Android export PASS
- [ ] Final installable artifact built if required
- [ ] Artifact source SHA verified

---

# Backend submission record

```text
Repository: Minimin0/ALLOG-Backend
Branch: main
Final commit SHA: TBD
Tag: hackathon-final-2026

Production API: https://api.allog-app.store
JAR SHA-256: TBD
Deployment timestamp: TBD
Previous JAR backup: TBD
Environment backup: TBD
Database backup: TBD / NOT_REQUIRED
Flyway version: TBD
```

### Backend final gate

- [ ] `git rev-parse HEAD` recorded
- [ ] working tree clean
- [ ] H2/full test PASS
- [ ] MySQL 8 test PASS
- [ ] concurrency PASS / skip 0
- [ ] bootJar PASS
- [ ] secret scan PASS
- [ ] Production deployment PASS
- [ ] systemd PASS
- [ ] nginx/HTTPS PASS
- [ ] Production auth smoke PASS

---

# Real app ↔ Production integration

```text
Frontend SHA: TBD
Backend SHA: TBD
Frontend artifact SHA-256: TBD
Backend JAR SHA-256: TBD
API: https://api.allog-app.store
Device / emulator: TBD
Android version: TBD
Tester: TBD
Timestamp: TBD
```

| Scenario | Result | Evidence / note |
| --- | --- | --- |
| Cold start | ☐ | |
| Login → HOME | ☐ | |
| Session restart | ☐ | |
| Explore | ☐ | |
| Group | ☐ | |
| My Page | ☐ | |
| Edit Profile | ☐ | |
| Profile save / server reflection | ☐ | |
| AI Coach | ☐ | |
| Verification | ☐ | |
| Reward / stats | ☐ | |
| Logout | ☐ | |
| Re-login | ☐ | |

Final integration result:

```text
TBD
```

Allowed final values:

- `PASS`
- `FAIL`
- `BLOCKED`

---

# Tag verification

## Frontend

```text
Tag: hackathon-final-2026
Tag target SHA: TBD
Matches Final commit SHA: TBD
```

## Backend

```text
Tag: hackathon-final-2026
Tag target SHA: TBD
Matches Final commit SHA: TBD
```

규칙: 제출 이후 기존 tag를 다른 SHA로 이동하지 않습니다.

---

# Submission package

```text
Submission timestamp: TBD
Submission destination: TBD
Demo/review account prepared: TBD
Credentials shared through private channel: TBD
README verified: TBD
Production API healthy: TBD
Final unresolved P0: 0 / TBD
Final unresolved P1: 0 / TBD
```

## Final declaration

아래 문구는 모든 gate가 통과한 뒤에만 사용합니다.

```text
ALLOG HACKATHON SUBMISSION FREEZE

Frontend:
<FINAL_FRONTEND_SHA>

Backend:
<FINAL_BACKEND_SHA>

Tag:
hackathon-final-2026

Integration:
PASS

Verdict:
READY_FOR_HACKATHON_SUBMISSION
```
