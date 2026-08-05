# API Collaboration Policy

백엔드 API 명세의 최종 기준은 OpenAPI 또는 Swagger로 관리합니다.

## 기본 원칙

- 프론트엔드는 확정된 API 명세를 기준으로 개발합니다.
- API 변경 시 백엔드 PR에 변경 전/후 명세를 작성합니다.
- Breaking Change 여부를 명확히 표시합니다.
- 요청 필드, 응답 필드, 상태 코드 변경을 기록합니다.
- 프론트엔드 영향 범위를 작성합니다.
- API가 완성되지 않은 경우 Mock 데이터 또는 Mock Server를 사용합니다.
- 운영 API 주소와 로컬 API 주소를 분리합니다.
- API 버전 경로는 `/api/v1` 형식을 우선 검토합니다.
- 인증이 필요한 API와 공개 API를 구분합니다.
- 에러 응답 형식은 공통화할 예정입니다.

## API 변경 사항

### 변경 전

`GET /api/challenges/{id}`

### 변경 후

`GET /api/v1/challenges/{challengeId}`

### 영향 범위

- 프론트엔드 챌린지 상세 조회 주소 수정 필요
- 기존 클라이언트와 호환되지 않는 Breaking Change
