# ALLOG Final Release Checklist

이 문서는 해커톤 제출 직전 ALLOG의 **정확한 source → build → deploy → integration → submission** 흐름을 고정하기 위한 최종 릴리스 기준입니다.

## Release principle

최종 제출본은 단순히 `main` 브랜치라는 이름으로 정의하지 않습니다.

최종 제출본은 다음 4개가 일치해야 합니다.

1. Frontend frozen commit SHA
2. Backend frozen commit SHA
3. 해당 SHA에서 만든 최종 앱 artifact / Backend JAR
4. 실제 Production 통합 테스트 결과

하나라도 변경되면 필요한 release gate를 다시 수행합니다.

---

## Current candidates — NOT FROZEN

작성 시점 기준:

| Repository | main SHA | Note |
| --- | --- | --- |
| Frontend | `6c0c747cfbe0d07afa057f6320b4a3539771123f` | PR #18 이후 UI 관련 3 commits 추가. 최종 재검증 필요 |
| Backend | `0e27e033acb45a6e83c49b9f139fb352ddf51ade` | 현재 production 배포 baseline. 최종 gate에서 재검증 |

이 값은 **현재 후보**일 뿐 제출 SHA가 아닙니다.

---

# 1. Frontend 최종 기능 확정

팀원이 더 이상 기능·UI를 추가하지 않는 시점을 선언합니다.

확인:

- [ ] START / Login / Signup
- [ ] Onboarding
- [ ] Home
- [ ] Explore
- [ ] Group
- [ ] Reward
- [ ] My Page
- [ ] Edit Profile
- [ ] AI Coach
- [ ] Verification
- [ ] Logout / session restore
- [ ] 더 이상 필수 UI 수정 없음
- [ ] Firebase / Phone / SMS / OTP를 다시 넣지 않음
- [ ] fake Heart / Reward / profile / verification success 없음

기능 freeze 이후에는 P0/P1 중심으로만 수정합니다.

---

# 2. Backend 최종 기능 확정

확인:

- [ ] Local ID/password auth
- [ ] JWT / SecureStore client contract
- [ ] Profile / onboarding
- [ ] Heart
- [ ] Group lifecycle / join / invite
- [ ] Progress
- [ ] Verification media / result
- [ ] AI Coach
- [ ] Reward/stats 현재 MVP 범위
- [ ] Production 환경 변수 계약 확정
- [ ] 추가 Flyway migration 필요 여부 확정
- [ ] 더 이상 필수 Backend 기능 변경 없음

Backend가 business truth authority인 원칙을 유지합니다.

---

# 3. Frontend → main 병합

모든 최종 Frontend PR을 review 후 `main`에 병합합니다.

병합 완료 후:

```bash
git switch main
git pull --ff-only origin main
git status --short
git rev-parse HEAD
```

기록:

```text
FRONTEND_RELEASE_CANDIDATE_SHA=<SHA>
```

조건:

- working tree clean
- origin/main과 local main 동일
- unresolved PR conflict 없음
- 제출에 포함될 코드가 모두 main에 존재

### 중요: stale PR 정리

오래된 인증 구조나 superseded 작업을 담은 PR이 실수로 merge되지 않도록 제출 전 open PR을 검토합니다.

특히 과거 Firebase 기반 integration branch처럼 현재 auth architecture와 충돌하는 PR은 **merge 금지** 상태를 명확히 합니다.

---

# 4. Backend → main 병합

모든 최종 Backend PR을 review 후 `main`에 병합합니다.

```bash
git switch main
git pull --ff-only origin main
git status --short
git rev-parse HEAD
```

기록:

```text
BACKEND_RELEASE_CANDIDATE_SHA=<SHA>
```

조건:

- working tree clean
- origin/main과 local main 동일
- migration ordering 정상
- 운영 secret commit 없음
- P0/P1 unresolved 없음

---

# 5. Frontend main Build/Test

**반드시 최종 main SHA에서 실행합니다.**

```bash
git switch main
git pull --ff-only origin main
git rev-parse HEAD
npm ci
node src/services/api.check.mjs
node src/stores/authStore.check.mjs
node src/stores/onboardingStore.check.mjs
node scripts/canonical-font.check.mjs
node scripts/canonical-ui-runtime.check.mjs
node scripts/video-frame-verification.check.mjs
git diff --check
```

Android export gate:

```bash
npx expo export \
  --platform android \
  --clear \
  --output-dir <SYSTEM_TEMP_DIRECTORY>
```

확인:

- [ ] 모든 check PASS
- [ ] production API URL 사용 가능
- [ ] secret scan PASS
- [ ] export artifact를 repository 안에 commit하지 않음
- [ ] latest UI 확인
- [ ] My → Edit Profile crash 없음

`expo export` 성공은 APK/AAB 생성 완료를 의미하지 않습니다. 최종 제출에서 설치 artifact가 필요하면 7단계에서 별도로 빌드합니다.

---

# 6. Backend main Build/Test

**반드시 최종 main SHA에서 실행합니다.**

기본 gate:

```bash
git switch main
git pull --ff-only origin main
git rev-parse HEAD
./gradlew test
./gradlew bootJar
git diff --check
```

최종 release에서는 프로젝트가 사용해 온 MySQL 8 / concurrency 검증도 다시 확인합니다.

확인:

- [ ] H2/full test PASS
- [ ] MySQL 8 integration PASS
- [ ] concurrency suites 실제 실행, skip 0
- [ ] bootJar PASS
- [ ] Flyway migration validation PASS
- [ ] auth / rate limiter PASS
- [ ] AI Coach tests PASS
- [ ] secret scan PASS

실패하면 배포하지 않습니다.

---

# 7. Frontend main으로 최종 앱 빌드

빌드 직전에 다시 SHA 확인:

```bash
git rev-parse HEAD
```

이 SHA가 `FRONTEND_RELEASE_CANDIDATE_SHA`와 같아야 합니다.

최종 artifact가 APK/AAB라면 팀이 선택한 Expo/EAS/native release pipeline으로 **이 SHA에서만** 생성합니다.

기록할 것:

```text
Frontend source SHA:
Artifact filename:
Artifact type: APK / AAB / other
Artifact SHA-256:
Build timestamp:
Build environment:
Production API: https://api.allog-app.store
```

규칙:

- dev-only localhost API 금지
- `adb reverse`에 의존한 제출 artifact 금지
- `10.0.2.2` production artifact 금지
- production API는 `https://api.allog-app.store`

---

# 8. Backend main으로 서버 최종 재배포

배포 artifact 역시 frozen Backend candidate SHA에서 생성합니다.

배포 전 기록:

```text
Backend source SHA:
JAR SHA-256:
Current production JAR backup path:
Environment backup path:
DB backup path (migration/DB change가 있으면 mandatory):
```

배포 후 확인:

- [ ] systemd `allog` active
- [ ] restart loop 없음
- [ ] Spring Boot startup PASS
- [ ] Flyway expected version PASS
- [ ] nginx PASS
- [ ] HTTPS PASS
- [ ] recent application error 없음
- [ ] protected API unauthenticated 401
- [ ] signup/login 정상
- [ ] `/users/me` 정상

Production 서버의 실제 source 기준이 `BACKEND_RELEASE_CANDIDATE_SHA`와 동일해야 합니다.

---

# 9. 실제 앱 ↔ 서버 통합 테스트

최종 앱 artifact와 최종 Production Backend를 연결하여 수행합니다.

환경:

```text
Frontend source SHA: <FINAL FRONTEND CANDIDATE>
Backend source SHA: <FINAL BACKEND CANDIDATE>
Production API: https://api.allog-app.store
Local Backend: OFF
Docker MySQL: NOT USED BY CLIENT
adb reverse: OFF
```

## Mandatory flow

- [ ] Cold start
- [ ] Login
- [ ] HOME
- [ ] Session restart / restore
- [ ] Explore
- [ ] Group
- [ ] My Page
- [ ] Edit Profile open
- [ ] Edit Profile save + real server reflection
- [ ] AI Coach
- [ ] Verification entry
- [ ] Camera permission only
- [ ] JPEG-only verification boundary
- [ ] Reward/stats real data
- [ ] Logout
- [ ] Back navigation으로 protected screen 복귀 안 됨
- [ ] Re-login

## Data / authority checks

- [ ] Heart = backend value
- [ ] Reward = backend value
- [ ] Group lifecycle = backend value
- [ ] AI facts = backend response
- [ ] Verification final result = backend authority
- [ ] fake local success 없음

## Failure rule

P0/P1 발견:

1. 제출 중지
2. scoped fix branch
3. PR → main
4. SHA 변경 기록
5. 영향받은 Build/Test 재실행
6. artifact 재빌드 또는 Backend 재배포
7. 통합 테스트 재실행

**기존 PASS 기록을 새 SHA에 재사용하지 않습니다.**

---

# Submission freeze point

9단계가 모두 PASS한 직후:

```bash
# Frontend repository
git switch main
git pull --ff-only origin main
git rev-parse HEAD

# Backend repository
git switch main
git pull --ff-only origin main
git rev-parse HEAD
```

두 SHA를 `SUBMISSION_RECORD.md`에 기록합니다.

이 순간 이후에는 코드를 수정하지 않는 것이 원칙입니다.

---

# Tag creation

추천 canonical tag:

```text
hackathon-final-2026
```

Frontend와 Backend에서 각각 실행:

```bash
git tag -a hackathon-final-2026 <FINAL_SHA> \
  -m "ALLOG hackathon final submission 2026"
git push origin hackathon-final-2026
```

검증:

```bash
git rev-list -n 1 hackathon-final-2026
git rev-parse HEAD
```

두 값이 동일해야 합니다.

### Tag immutability

제출 후에는 `hackathon-final-2026` 태그를 다른 SHA로 이동시키지 않습니다.

추가 버전이 정말 필요하면 예:

```text
hackathon-final-2026.1
```

처럼 새 태그를 생성합니다.

---

# 10. 제출

제출 직전 마지막으로 기록:

```text
ALLOG-Frontend
repository: Minimin0/ALLOG-Frontend
branch: main
commit: <FINAL_FRONTEND_SHA>
tag: hackathon-final-2026
artifact: <NAME>
artifact sha256: <SHA256>

ALLOG-Backend
repository: Minimin0/ALLOG-Backend
branch: main
commit: <FINAL_BACKEND_SHA>
tag: hackathon-final-2026
production: https://api.allog-app.store
jar sha256: <SHA256>
```

그리고 제출물, README, 시연용 계정, Production 서버가 모두 이 기록과 일치하는지 확인합니다.

---

# Final verdict

다음 조건이 모두 참일 때만:

```text
READY_FOR_HACKATHON_SUBMISSION
```

- Frontend final main test PASS
- Backend final main test PASS
- Final app artifact built from recorded Frontend SHA
- Production Backend deployed from recorded Backend SHA
- Real app ↔ Production integration PASS
- No P0/P1 unresolved
- Submission Record complete
- Tags point to exact submitted SHAs
