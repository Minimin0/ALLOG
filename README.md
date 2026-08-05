# ALLOG

ALLOG는 같은 웰니스 목표를 가진 사용자가 무료 활동으로 획득한 하트를 사용해 그룹 챌린지에 참가하고, AI 코칭과 그룹 경쟁을 통해 루틴을 지속하며 리워드 포인트를 획득하는 그룹형 AI 웰니스 서비스입니다.

이 저장소는 ALLOG 프로젝트의 공통 문서 및 저장소 안내를 관리하는 허브입니다.

## Repositories

- Frontend: https://github.com/Minimin0/ALLOG-Frontend
- Backend: https://github.com/Minimin0/ALLOG-Backend
- Project Docs: 현재 저장소의 `docs/`

## Repository Responsibilities

### ALLOG

공통 기획, 시스템 설계, 회의 기록, 개발 정책을 관리합니다.

### ALLOG-Frontend

모바일 앱 또는 웹 UI, 사용자 인터랙션, 상태 관리와 API 연동을 관리합니다.

### ALLOG-Backend

API, 데이터베이스, 인증, 비즈니스 로직, AI 연동과 서버 배포를 관리합니다.

## Document Structure

- `docs/planning/`: 기획서와 기능 범위
- `docs/architecture/`: 시스템 설계
- `docs/meetings/`: 회의 기록
- `docs/policies/`: API, 브랜치, 리뷰 등 개발 정책

기존 `frontend/`와 `backend/` 폴더는 저장소 분리 이전 안내를 보존하고 새 저장소 링크를 제공합니다.

## Branch Strategy

```text
main
└── develop
    ├── feature/*
    ├── fix/*
    ├── refactor/*
    ├── test/*
    └── docs/*
```

브랜치 이름 예시:

- `feature/login`
- `feature/challenge-list`
- `feature/auth-api`
- `feature/ranking-api`
- `fix/token-expiration`
- `fix/navigation-error`
- `docs/api-spec`
- `refactor/challenge-service`

## Commit Convention

- `feat`: 새로운 기능
- `fix`: 버그 수정
- `docs`: 문서 수정
- `style`: 코드 포맷 변경
- `refactor`: 리팩터링
- `test`: 테스트 추가 또는 수정
- `chore`: 설정 및 기타 작업

## Pull Request Rules

1. 기능별 브랜치에서 작업합니다.
2. `develop` 브랜치로 Pull Request를 생성합니다.
3. 자기 자신이 작성한 PR도 변경 내용을 직접 검토합니다.
4. 가능한 경우 최소 1명의 리뷰를 받은 후 병합합니다.
5. API 변경 사항은 PR 본문에 반드시 작성합니다.
6. 테스트하지 않은 기능을 테스트 완료로 표시하지 않습니다.
7. `main`에는 직접 푸시하지 않습니다.
8. 배포 가능한 버전만 `develop`에서 `main`으로 병합합니다.
