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

## Current Release Candidate

> 아래 SHA는 **현재 main 기준 후보값**입니다. 아직 제출본으로 freeze된 값이 아닙니다. 최종 통합 테스트가 끝난 뒤 Submission Record에 다시 기록합니다.

| Repository | Branch | Current candidate SHA | Status |
| --- | --- | --- | --- |
| ALLOG-Frontend | `main` | `6c0c747cfbe0d07afa057f6320b4a3539771123f` | Final re-test required |
| ALLOG-Backend | `main` | `0e27e033acb45a6e83c49b9f139fb352ddf51ade` | Final re-test / deploy gate required |

Frontend `main`은 PR #18 이후 UI 관련 커밋이 추가되었으므로 이전 QA 결과만으로 제출본을 확정하지 않습니다. 최종 제출 전 반드시 현재 `main`에서 다시 Build/Test 합니다.

## Final Release Gate

제출 전 작업 순서는 아래 10단계를 고정 기준으로 사용합니다.

- [ ] 1. Frontend 최종 기능 확정
- [ ] 2. Backend 최종 기능 확정
- [ ] 3. Frontend 변경사항을 `main`에 병합
- [ ] 4. Backend 변경사항을 `main`에 병합
- [ ] 5. Frontend `main` Build/Test
- [ ] 6. Backend `main` Build/Test
- [ ] 7. Frontend `main`으로 최종 앱 artifact 빌드
- [ ] 8. Backend `main`으로 서버 최종 재배포
- [ ] 9. 실제 앱 ↔ Production 서버 통합 테스트
- [ ] 10. 제출 및 제출본 SHA/태그 기록

상세 명령, 실패 시 되돌아갈 단계, 통합 테스트 기준은 [`docs/release/FINAL_RELEASE_CHECKLIST.md`](docs/release/FINAL_RELEASE_CHECKLIST.md)를 따릅니다.

## Submission Freeze

최종 통합 테스트가 모두 통과한 순간 두 저장소의 SHA를 기록합니다.

```text
ALLOG-Frontend
branch: main
commit: <FINAL_FRONTEND_SHA>

tag: hackathon-final-2026

ALLOG-Backend
branch: main
commit: <FINAL_BACKEND_SHA>

tag: hackathon-final-2026
```

정확한 제출 기록은 [`docs/release/SUBMISSION_RECORD.md`](docs/release/SUBMISSION_RECORD.md)에 남깁니다.

### Tag policy

- 태그는 **9번 실제 앱 ↔ 서버 통합 테스트까지 PASS한 뒤** 생성합니다.
- 동일한 태그 이름 `hackathon-final-2026`을 Frontend와 Backend 각각의 저장소에 생성합니다.
- 태그는 저장소별 객체이므로 동일한 이름이어도 각각 다른 최종 SHA를 정확히 가리킬 수 있습니다.
- 제출 후 기존 태그를 강제로 다른 커밋으로 이동하지 않습니다.
- 제출 전 추가 수정이 생기면 새로운 SHA에서 Build/Test/통합 테스트를 다시 수행한 뒤 태그를 생성합니다.
- 제출 후 별도 hotfix를 기록해야 한다면 기존 태그를 움직이지 말고 새 태그를 사용합니다.

## Team Final Sync

### Frontend

```bash
git switch main
git pull --ff-only origin main
git rev-parse HEAD
git status --short
npm ci
EXPO_PUBLIC_API_BASE_URL=https://api.allog-app.store npx expo start --clear
```

팀원 모두 `git rev-parse HEAD` 결과가 최종 Frontend SHA와 같은지 확인한 뒤 검수합니다.

### Backend

```bash
git switch main
git pull --ff-only origin main
git rev-parse HEAD
git status --short
./gradlew test
./gradlew bootJar
```

배포 artifact는 반드시 최종 Backend SHA에서 생성합니다.

## Release Rules

1. 기능 freeze 이후에는 `main` 직접 수정 대신 작은 PR 단위로 마지막 변경을 관리합니다.
2. Frontend UI 변경이더라도 인증·API·서버 authoritative data flow를 회귀시키지 않습니다.
3. Backend는 Heart, 그룹 lifecycle, reward, verification 결과의 최종 authority입니다.
4. Firebase/전화번호/SMS/OTP 인증은 현재 production auth에 포함하지 않습니다.
5. 비밀번호, JWT signing secret, DB 비밀번호, AI API key, media signing secret 등 운영 secret은 Git에 기록하지 않습니다.
6. 테스트하지 않은 항목은 PASS로 표시하지 않습니다.
7. 최종 앱 artifact와 서버 배포물이 어떤 source SHA에서 생성되었는지 반드시 기록합니다.
8. 최종 SHA 이후 코드가 한 줄이라도 바뀌면 해당 영역의 Build/Test와 필요한 통합 테스트를 다시 수행합니다.

## Release Documents

- [`FINAL_RELEASE_CHECKLIST.md`](docs/release/FINAL_RELEASE_CHECKLIST.md) — 마감 전 10단계 릴리스 절차
- [`SUBMISSION_RECORD.md`](docs/release/SUBMISSION_RECORD.md) — 최종 SHA, 태그, build/deploy, 통합 테스트 증빙
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

## Collaboration Rules

1. 작업 전 최신 `main`을 기준으로 브랜치를 생성합니다.
2. 기능·버그 수정은 Frontend/Backend 각 저장소의 scoped branch에서 수행합니다.
3. 변경사항은 Pull Request로 검토한 뒤 `main`에 병합합니다.
4. 마감 직전에는 P0/P1만 수정하고 불필요한 리팩터링·의존성 대규모 변경은 피합니다.
5. 최종 제출 기준은 브랜치 이름이 아니라 **기록된 commit SHA + tag**입니다.
