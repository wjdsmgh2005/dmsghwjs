# 당근나라 PC 🥕

> 컴퓨터 주변기기를 동네에서 안전하게 거래하는 중고거래 플랫폼

---

## 스크린샷 / 데모

| 홈 피드 | 상품 상세 | 채팅 |
|--------|---------|------|
| 카테고리 필터 + 상품 목록 | 판매자 정보 + 채팅하기 | 1:1 실시간 채팅 |

> 데모 영상: `docs/demo.mp4` 참고

---

## 주요 기능

- 컴퓨터 주변기기 카테고리 전문 필터링 (GPU, 마우스, 키보드, 모니터 등)
- 상품 등록 / 탐색 / 상세 조회
- 판매자-구매자 1:1 채팅
- 관심 목록 (찜) 기능
- 매너온도 기반 판매자 신뢰 시스템
- 가격 협의 가능 태그
- 판매 상태 관리 (판매중 / 거래완료)

---

## 기술 스택

| 영역 | 기술 |
|------|------|
| 프레임워크 | React Native (Expo Managed) |
| 언어 | TypeScript |
| 상태 관리 | Zustand |
| 백엔드 | Firebase (Firestore, Auth, Storage) |
| 네비게이션 | React Navigation v6 |
| 로컬 저장 | AsyncStorage |
| 테스트 | Jest, @testing-library/react-native |

---

## 빠른 시작

```bash
git clone https://github.com/{본인계정}/danggeun-nara.git
cd danggeun-nara
npm install
cp .env.example .env
npx expo start
```

자세한 내용은 [docs/setup.md](docs/setup.md) 참고

---

## 빌드 / 배포

```bash
# Expo Go로 바로 실행
npx expo start

# APK 빌드
eas build --platform android

# iOS 빌드
eas build --platform ios
```

자세한 내용은 [docs/deploy.md](docs/deploy.md) 참고

---

## 테스트

```bash
# 전체 테스트 실행
npm test

# 커버리지 포함
npm test -- --coverage
```

자세한 내용은 [docs/testing.md](docs/testing.md) 참고

---

## 프로젝트 구조

```
src/
├── presentation/   # 화면 · 컴포넌트 · 테마
├── application/    # 상태관리 · 유스케이스
├── domain/         # 엔티티 · 비즈니스 규칙
└── data/           # API · 로컬 저장소
```

자세한 내용은 [docs/architecture.md](docs/architecture.md) 참고

---

## 라이선스

MIT License © 2026 당근나라 PC Team
