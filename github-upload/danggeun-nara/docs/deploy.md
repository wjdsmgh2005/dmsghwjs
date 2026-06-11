# 배포 가이드 — 당근나라 PC

---

## 빌드 종류

| 종류 | 용도 | 명령어 |
|------|------|--------|
| debug | 로컬 개발 | `npx expo start` |
| preview | 팀 내부 테스트 | `eas build --profile preview` |
| release | 실제 배포 | `eas build --profile production` |

---

## 빠른 실행 (Expo Go)

```bash
# 1. 의존성 설치
npm install

# 2. 환경 변수 설정
cp .env.example .env
# .env 파일에 Firebase 키 입력

# 3. 실행
npx expo start

# 4. QR코드를 Expo Go 앱으로 스캔
```

---

## APK 빌드 (Android)

```bash
# EAS CLI 설치
npm install -g eas-cli

# 로그인
eas login

# APK 빌드
eas build --platform android --profile preview

# 빌드 완료 후 다운로드 링크 제공됨
```

---

## iOS 빌드

```bash
# iOS 빌드 (Mac + Xcode 필요)
eas build --platform ios --profile preview
```

---

## 환경 분리

```
.env.dev      # 로컬 개발 (목 데이터, 로깅 verbose)
.env.staging  # 내부 테스트 (실 API, 디버그 가능)
.env.prod     # 실배포 (실 API, 디버그 차단)
.env.example  # Git에 커밋하는 샘플 파일
```

**⚠️ 주의:** `.env.dev`, `.env.staging`, `.env.prod` 는 `.gitignore`에 포함 — 절대 커밋 금지

---

## 환경변수 목록 (.env.example)

```env
EXPO_PUBLIC_FIREBASE_API_KEY=
EXPO_PUBLIC_FIREBASE_AUTH_DOMAIN=
EXPO_PUBLIC_FIREBASE_PROJECT_ID=
EXPO_PUBLIC_FIREBASE_STORAGE_BUCKET=
EXPO_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=
EXPO_PUBLIC_FIREBASE_APP_ID=
```

---

## 버전 관리 (SemVer)

```
MAJOR.MINOR.PATCH
1.0.0  → 최초 릴리스
1.1.0  → 새 기능 추가 (Should 기능)
1.1.1  → 버그 수정
```

---

## 롤백 방법

```bash
# 이전 커밋으로 되돌리기
git revert HEAD

# 특정 버전으로 되돌리기
git checkout v1.0.0

# EAS에서 이전 빌드 재배포
eas build:list  # 빌드 목록 확인
```

---

## 보안 체크리스트

- [ ] API 키가 코드에 하드코딩되어 있지 않은가
- [ ] `.gitignore`에 `.env`, 인증서, keystore 포함
- [ ] 사용자 입력 검증 완료
- [ ] 통신 HTTPS 강제
- [ ] 로컬 저장 민감정보 암호화
- [ ] 로그에 비밀번호 / 토큰 출력 안 됨
- [ ] 권한 (카메라, 위치, 알림) 사유 명시
