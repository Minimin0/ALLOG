# ALLOG Submission Record

> ALLOG 해커톤 제출본의 정확한 source SHA를 동결한 기록입니다.

## Submission status

```text
STATUS: FROZEN_FOR_SUBMISSION
```

Freeze date: `2026-08-21 KST`

사용자 확인: 최종 Frontend/Backend Build/Test 및 실제 앱 ↔ Production 서버 통합 검증 완료.

---

## Frozen source

| Component | Repository | Branch | Final SHA |
| --- | --- | --- | --- |
| Frontend | `Minimin0/ALLOG-Frontend` | `main` | `6c0c747cfbe0d07afa057f6320b4a3539771123f` |
| Backend | `Minimin0/ALLOG-Backend` | `main` | `0e27e033acb45a6e83c49b9f139fb352ddf51ade` |

이 두 SHA가 ALLOG 제출 소스의 기준입니다.

이후 코드가 변경되더라도 위 SHA는 제출 시점의 기준으로 유지합니다. 제출본을 다시 변경하려면 새로운 SHA에서 Build/Test/Production 통합 테스트를 다시 수행한 뒤 새로운 submission record를 작성합니다.

---

# Frontend submission record

```text
Repository: Minimin0/ALLOG-Frontend
Branch: main
Final commit SHA: 6c0c747cfbe0d07afa057f6320b4a3539771123f
Tag target: hackathon-final-2026
Production API: https://api.allog-app.store
Artifact filename: NOT_RECORDED_IN_HUB
Artifact SHA-256: NOT_RECORDED_IN_HUB
```

Final gate: `PASS — user-confirmed final verification`

---

# Backend submission record

```text
Repository: Minimin0/ALLOG-Backend
Branch: main
Final commit SHA: 0e27e033acb45a6e83c49b9f139fb352ddf51ade
Tag target: hackathon-final-2026
Production API: https://api.allog-app.store
JAR SHA-256: c3d06cf13c63cc421a35f67a434ea3acef0ba9cbad2b795d80365f3903aa6525
Production Flyway: V18
```

Final gate: `PASS — production deployment and auth/API smoke previously verified; user-confirmed final verification`

---

# Real app ↔ Production integration

```text
Frontend SHA: 6c0c747cfbe0d07afa057f6320b4a3539771123f
Backend SHA: 0e27e033acb45a6e83c49b9f139fb352ddf51ade
API: https://api.allog-app.store
Local Backend: OFF
Result: PASS
```

사용자 확인에 따라 최종 실제 앱 ↔ Production 검증을 완료한 것으로 freeze합니다.

필수 검수 범위:

- Cold start
- Login → HOME
- Session restart / restore
- Explore
- Group
- My Page
- Edit Profile
- Profile save / server reflection
- AI Coach
- Verification
- Reward / stats
- Logout
- Re-login

---

# Tag policy

Canonical tag name:

```text
hackathon-final-2026
```

Expected targets:

```text
ALLOG-Frontend
hackathon-final-2026
→ 6c0c747cfbe0d07afa057f6320b4a3539771123f

ALLOG-Backend
hackathon-final-2026
→ 0e27e033acb45a6e83c49b9f139fb352ddf51ade
```

제출 후 기존 tag를 다른 SHA로 이동하지 않습니다.

> 현재 ChatGPT GitHub 연결은 tag-ref 생성 동작을 제공하지 않으므로 실제 Git tag 생성 여부는 별도로 검증해야 합니다. 태그가 없더라도 위 full SHA가 제출 소스의 authoritative freeze record입니다.

---

# Submission declaration

```text
ALLOG HACKATHON SUBMISSION FREEZE

Frontend:
6c0c747cfbe0d07afa057f6320b4a3539771123f

Backend:
0e27e033acb45a6e83c49b9f139fb352ddf51ade

Production API:
https://api.allog-app.store

Integration:
PASS

P0:
0

P1:
0

Verdict:
FROZEN_FOR_SUBMISSION
```
