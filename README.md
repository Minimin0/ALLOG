# ALLOG

ALLOG 앱의 프론트엔드와 백엔드 개발을 위한 팀 공동 저장소입니다.

## Repository Structure

- `frontend/`: 모바일 앱 또는 웹 프론트엔드 소스 코드
- `backend/`: API, 데이터베이스 및 서버 애플리케이션 소스 코드
- `docs/`: 기획서, API 명세, 시스템 설계 및 개발 문서

## Branch Strategy

- `main`: 배포 가능한 안정 버전
- `develop`: 개발 기능이 통합되는 브랜치
- `feature/frontend-*`: 프론트엔드 기능 개발
- `feature/backend-*`: 백엔드 기능 개발
- `fix/*`: 버그 수정
- `docs/*`: 문서 작업

## Collaboration Rules

1. `main` 브랜치에 직접 커밋하지 않습니다.
2. 모든 기능은 별도의 브랜치에서 작업합니다.
3. 작업 완료 후 Pull Request를 생성합니다.
4. 코드 리뷰 후 `develop` 브랜치에 병합합니다.
5. 배포 가능한 상태가 되면 `develop`에서 `main`으로 Pull Request를 생성합니다.

## Commit Convention

- `feat`: 새로운 기능
- `fix`: 버그 수정
- `docs`: 문서 수정
- `style`: 코드 포맷 수정
- `refactor`: 코드 리팩터링
- `test`: 테스트 추가 또는 수정
- `chore`: 설정 및 기타 작업
