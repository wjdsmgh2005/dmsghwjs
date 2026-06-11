# Setup Guide — 당근나라

> 이 문서만 보고 **5분 안에 실행**할 수 있어야 합니다.

---

## 필요한 도구

| 도구 | 버전 | 확인 명령 |
|------|------|-----------|
| Node.js | 20.x LTS | `node -v` |
| npm | 10.x | `npm -v` |
| Expo CLI | 최신 | `npx expo --version` |
| Git | 2.x | `git --version` |

> iOS 실기기 테스트: **Expo Go** 앱 (App Store)  
> Android 실기기 테스트: **Expo Go** 앱 (Play Store)

---

## 1. 클론

```bash
git clone https://github.com/{본인계정}/danggeun-nara.git
cd danggeun-nara
```

---

## 2. 의존성 설치

```bash
npm install
```

---

## 3. 환경 변수 설정

```bash
cp .env.example .env
```

`.env` 파일을 열고 Firebase 값을 채워넣으세요:

```env
EXPO_PUBLIC_FIREBASE_API_KEY=your_api_key
EXPO_PUBLIC_FIREBASE_AUTH_DOMAIN=your_project.firebaseapp.com
EXPO_PUBLIC_FIREBASE_PROJECT_ID=your_project_id
EXPO_PUBLIC_FIREBASE_STORAGE_BUCKET=your_project.appspot.com
EXPO_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=000000000000
EXPO_PUBLIC_FIREBASE_APP_ID=1:000000000000:web:xxxxxxxxxxxxxxxx
```

> Firebase 콘솔 → 프로젝트 설정 → 앱 추가에서 값 확인

---

## 4. 첫 실행

```bash
npx expo start
```

터미널에 QR코드가 뜨면:
- **실기기**: Expo Go 앱으로 QR 스캔
- **Android 에뮬레이터**: `a` 키 입력
- **iOS 시뮬레이터** (Mac 전용): `i` 키 입력

---

## 플랫폼별 주의사항

### Windows
- Android Studio 에뮬레이터 권장
- iOS 시뮬레이터 불가 (Mac 전용)

### macOS
- Xcode 설치 후 iOS 시뮬레이터 가능
- `sudo xcode-select --install`

### Linux
- Android 에뮬레이터 또는 실기기 사용

---

## FAQ (문제 해결)

**Q1. `npx expo start`가 안 돼요**
```bash
npm install -g expo-cli
npx expo start --clear
```

**Q2. 패키지 설치 중 오류가 나요**
```bash
rm -rf node_modules package-lock.json
npm install
```

**Q3. Metro 번들러가 멈춰요**
```bash
npx expo start --clear
# 혹은 Ctrl+C 후 재시작
```

**Q4. 실기기에서 앱이 안 열려요**
- 개발 PC와 실기기가 **같은 Wi-Fi**에 연결되어 있는지 확인
- Expo Go 앱 최신 버전인지 확인

**Q5. Firebase 연결 오류가 나요**
- `.env` 파일의 값이 정확한지 확인
- Firebase 콘솔에서 앱 등록이 완료되었는지 확인
- Firestore 데이터베이스가 "테스트 모드"로 생성되었는지 확인
