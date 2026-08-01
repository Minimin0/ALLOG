# Contributing

ALLOG 저장소는 프론트엔드와 백엔드 개발자가 함께 사용하는 팀 저장소입니다. 모든 작업은 `develop` 브랜치를 기준으로 기능 브랜치를 만들어 진행합니다.

## 저장소 클론

```bash
git clone https://github.com/Minimin0/ALLOG.git
cd ALLOG
```

## 작업 브랜치 생성

```bash
git checkout develop
git pull origin develop
git checkout -b feature/frontend-login
```

브랜치 이름 예시:

- `feature/frontend-login`
- `feature/backend-auth-api`
- `fix/login-validation`
- `docs/api-spec`

## 커밋 메시지

```bash
git commit -m "feat: 로그인 화면 구현"
```

커밋 타입 예시:

- `feat`: 새로운 기능
- `fix`: 버그 수정
- `docs`: 문서 수정
- `style`: 코드 포맷 수정
- `refactor`: 코드 리팩터링
- `test`: 테스트 추가 또는 수정
- `chore`: 설정 및 기타 작업

## Pull Request

1. 작업 브랜치에서 변경 사항을 커밋합니다.
2. 원격 저장소에 브랜치를 푸시합니다.
3. GitHub에서 `develop` 브랜치로 Pull Request를 생성합니다.
4. 병합 전 코드 리뷰를 권장합니다.
5. 리뷰가 끝나면 `develop` 브랜치에 병합합니다.

## 보안 규칙

- `main` 브랜치에 직접 푸시하지 않습니다.
- `.env`, API Key, 비밀번호 등 비밀정보를 커밋하지 않습니다.
- 비밀정보가 필요한 경우 로컬 환경 변수 또는 별도 비밀 관리 도구를 사용합니다.
