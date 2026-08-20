# ALLOG

> 같은 목표를 가진 사람들이 그룹 챌린지에 참여하고, AI 코칭과 인증을 통해 웰니스 루틴을 지속하는 그룹형 웰니스 앱입니다.

ALLOG의 공통 기획·개발·릴리스 문서를 관리하는 프로젝트 허브입니다. 실제 애플리케이션 코드는 Frontend와 Backend 저장소에서 각각 관리합니다.

## Repositories

| Area | Repository | Role |
| --- | --- | --- |
| Frontend | [Minimin0/ALLOG-Frontend](https://github.com/Minimin0/ALLOG-Frontend) | React Native / Expo Router Android-first client |
| Backend | [Minimin0/ALLOG-Backend](https://github.com/Minimin0/ALLOG-Backend) | Java 21 / Spring Boot / MySQL API server |
| Project Hub | [Minimin0/ALLOG](https://github.com/Minimin0/ALLOG) | 기획, 공통 문서, 최종 릴리스·제출 기록 |

Production API: `https://api.allog-app.store`

## Submission Freeze

**Status: `FROZEN_FOR_SUBMISSION`**

최종 Build/Test와 실제 앱 ↔ Production 통합 검증 완료를 기준으로 아래 SHA를 제출 소스로 동결합니다.

| Repository | Branch | Frozen SHA |
| --- | --- | --- |
| ALLOG-Frontend | `main` | `6c0c747cfbe0d07afa057f6320b4a3539771123f` |
| ALLOG-Backend | `main` | `0e27e033acb45a6e83c49b9f139fb352ddf51ade` |

이후 `main`이 변경되더라도 제출 당시 기준은 위 full SHA입니다.

Canonical tag name: `hackathon-final-2026`

- Frontend tag target: `6c0c747cfbe0d07afa057f6320b4a3539771123f`
- Backend tag target: `0e27e033acb45a6e83c49b9f139fb352ddf51ade`
- 제출 후 기존 태그를 다른 SHA로 이동하지 않습니다.

정확한 제출 기록은 [`docs/release/SUBMISSION_RECORD.md`](docs/release/SUBMISSION_RECORD.md)에 남깁니다.

## Final Release Gate

제출 전 기준 10단계는 모두 완료된 것으로 freeze합니다.

- [x] 1. Frontend 최종 기능 확정
- [x] 2. Backend 최종 기능 확정
- [x] 3. Frontend 변경사항 `main` 반영
- [x] 4. Backend 변경사항 `main` 반영
- [x] 5. Frontend `main` Build/Test
- [x] 6. Backend `main` Build/Test
- [x] 7. Frontend 최종 앱 검증
- [x] 8. Backend production 배포 검증
- [x] 9. 실제 앱 ↔ Production 서버 통합 테스트
- [x] 10. 제출용 SHA freeze 기록

상세 절차는 [`docs/release/FINAL_RELEASE_CHECKLIST.md`](docs/release/FINAL_RELEASE_CHECKLIST.md)를 따릅니다.

## Architecture Baseline

```text
React Native / Expo Router Frontend
        ↓ HTTPS
https://api.allog-app.store
        ↓
Gabia nginx
        ↓
Spring Boot / Java 21 Backend
        ↓
MySQL 8
```

### Production rules

- 인증: Local ID + Password
- Client token storage: Expo SecureStore
- Firebase Auth: 사용하지 않음
- Phone / SMS / OTP: 사용하지 않음
- Heart / Reward / Group lifecycle / Verification result: Backend authority
- Verification network media: JPEG/PNG only
- Production API: `https://api.allog-app.store`

## Team Final Sync

### Frontend

```bash
git switch main
git pull --ff-only origin main
git rev-parse HEAD
npm ci
EXPO_PUBLIC_API_BASE_URL=https://api.allog-app.store npx expo start --clear
```

제출본 재현이 필요하면 아래 SHA를 checkout합니다.

```bash
git checkout 6c0c747cfbe0d07afa057f6320b4a3539771123f
```

### Backend

```bash
git switch main
git pull --ff-only origin main
git rev-parse HEAD
./gradlew test
./gradlew bootJar
```

제출본 재현이 필요하면 아래 SHA를 checkout합니다.

```bash
git checkout 0e27e033acb45a6e83c49b9f139fb352ddf51ade
```

## Release Rules

1. 제출 기준은 브랜치 이름이 아니라 기록된 **full commit SHA**입니다.
2. 최종 SHA 이후 코드 변경은 제출본을 자동 변경하지 않습니다.
3. 제출본을 변경하려면 새 SHA에서 Build/Test/통합 검증을 다시 수행해야 합니다.
4. Frontend는 Backend authoritative data flow를 임의로 대체하지 않습니다.
5. 비밀번호, JWT signing secret, DB 비밀번호, AI API key, media signing secret 등 운영 secret은 Git에 기록하지 않습니다.
6. Firebase/전화번호/SMS/OTP 인증을 현재 production auth에 다시 도입하지 않습니다.
7. 태그를 만든 뒤에는 기존 `hackathon-final-2026` 태그를 다른 SHA로 강제 이동하지 않습니다.

## Release Documents

- [`FINAL_RELEASE_CHECKLIST.md`](docs/release/FINAL_RELEASE_CHECKLIST.md) — 마감 전 릴리스 절차 및 재현 기준
- [`SUBMISSION_RECORD.md`](docs/release/SUBMISSION_RECORD.md) — 최종 SHA, 태그 target, production 통합 결과
- [`CONTRIBUTING.md`](CONTRIBUTING.md) — 공통 협업 규칙

## Repository Structure

```text
.
├── docs/
│   └── release/
│       ├── FINAL_RELEASE_CHECKLIST.md
│       └── SUBMISSION_RECORD.md
├── CONTRIBUTING.md
└── README.md
```
