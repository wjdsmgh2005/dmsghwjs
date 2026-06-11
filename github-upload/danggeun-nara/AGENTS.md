# AGENTS.md — 당근나라 PC AI 에이전트 정책 통합 파일

> 작성자: 전은호 | 2024136081 | 3학년 C반  
> 프로젝트: 당근나라 PC (컴퓨터 주변기기 중고거래 사기방지 플랫폼)  
> 최종 수정: 2025-06

---

## 1. 프로젝트 개요 (Agent Context)

이 저장소는 **중고거래 사기 방지**에 특화된 모바일 앱 프로젝트입니다.  
AI Agent가 이 파일을 읽으면 프로젝트의 목적, 구조, 규칙을 즉시 파악할 수 있습니다.

```
당근나라 PC
├── 비전: 컴퓨터 주변기기 전문 중고거래에서 사기를 원천 차단한다
├── 핵심 차별점: 에스크로·신뢰등급·AI사기탐지·안전점수
├── 기술: React Native (Expo) · Firebase · Zustand
└── 데모: danggeun-nara.html (localStorage 백엔드)
```

---

## 2. AI Agent 규칙 (Rules)

### 2.1 코드 생성 규칙
- 모든 컴포넌트는 `src/presentation/screens/` 또는 `src/presentation/components/` 에 위치
- 비즈니스 로직은 `src/application/viewModels/` 에만 작성, UI에 직접 작성 금지
- Firebase 호출은 반드시 `src/data/repositories/` 를 통해서만 수행
- 하드코딩된 문자열은 `src/constants/` 에 상수로 정의
- 컴포넌트 props는 TypeScript interface로 반드시 정의

### 2.2 안전 관련 규칙 (핵심)
- 사기 탐지 키워드 목록은 `src/constants/fraudKeywords.ts` 에서만 관리
- 에스크로 상태 전이 로직은 `src/domain/services/EscrowService.ts` 에서만 수정
- 신뢰등급 계산 로직 변경 시 반드시 단위 테스트 업데이트 필요
- 신고 관련 기능은 `src/application/viewModels/ReportViewModel.ts` 에서 처리

### 2.3 금지 사항
- `presentation` 레이어에서 Firebase 직접 호출 금지
- `data` 레이어에서 UI 상태 변경 금지
- 개인정보(전화번호, 계좌번호) 를 로컬 스토리지에 저장 금지
- 암호화되지 않은 민감 데이터를 AsyncStorage에 저장 금지

---

## 3. Skills (AI 작업 단위)

### skill: create-screen
새 화면을 만들 때 자동으로 적용되는 템플릿
```
1. src/presentation/screens/{ScreenName}Screen.tsx 생성
2. src/application/viewModels/{ScreenName}ViewModel.ts 생성
3. src/navigation/AppNavigator.tsx 에 라우트 추가
4. 해당 테스트 파일 src/__tests__/{ScreenName}.test.ts 생성
```

### skill: create-repository
새 데이터 소스 연결 시 적용
```
1. src/data/repositories/{Entity}Repository.ts 생성 (interface)
2. src/data/repositories/firebase/{Entity}FirebaseRepository.ts 구현
3. src/domain/entities/{Entity}.ts 도메인 모델 정의
```

### skill: add-safety-feature
안전 기능 추가 시 체크리스트
```
□ fraudKeywords.ts 업데이트 여부 확인
□ SafetyScoreService 점수 계산 로직 영향 확인
□ 에스크로 플로우와 충돌 없는지 확인
□ 단위 테스트 추가
□ ADR 작성 (아키텍처 결정 사항이면)
```

### skill: fix-bug
버그 수정 시 적용 순서
```
1. 재현 조건 파악 → 테스트 케이스 먼저 작성
2. 최소 범위 수정
3. lessons/ 에 디버깅 사례 기록
4. 관련 ADR이 있으면 업데이트
```

---

## 4. Slash Commands

| 명령어 | 동작 |
|--------|------|
| `/new-screen [이름]` | 화면 + ViewModel + 테스트 파일 일괄 생성 |
| `/new-repo [엔티티]` | Repository interface + Firebase 구현체 생성 |
| `/adr [제목]` | ADR 템플릿 생성 및 decisions/ 에 저장 |
| `/safety-check` | 안전 기능 체크리스트 실행 |
| `/test-run` | Jest 단위 테스트 + 통합 테스트 실행 |
| `/build-apk` | Expo EAS Build Android APK 트리거 |
| `/deploy-preview` | Expo Go 미리보기 링크 생성 |
| `/lesson [제목]` | lessons/ 디버깅 사례 파일 생성 |

---

## 5. 워크플로우 (Workflows)

### workflow: feature-development
```
요구사항 확인
    → ADR 작성 (아키텍처 결정 필요 시)
    → /new-screen or /new-repo 실행
    → 구현
    → /safety-check (안전 기능이면)
    → /test-run
    → PR 생성
    → lessons/ 기록
```

### workflow: bug-fix
```
이슈 확인
    → 재현 테스트 작성
    → /fix-bug 실행
    → 테스트 통과 확인
    → /lesson 으로 디버깅 사례 기록
```

### workflow: release
```
/test-run 전체 통과 확인
    → /build-apk
    → 빌드 결과 docs/deploy.md 업데이트
    → GitHub Release 태그 생성
    → /deploy-preview 링크 README에 업데이트
```

---

## 6. 디렉토리 구조 규칙

```
danggeun-nara/
├── AGENTS.md              ← 이 파일 (Agent 정책 통합)
├── README.md              ← 설치/실행 가이드
├── BONUS.md               ← 가산점 내역
├── danggeun-nara.html     ← 웹 데모 (localStorage 백엔드)
├── src/
│   ├── presentation/      ← UI 레이어 (React Native 컴포넌트)
│   │   ├── screens/       ← 화면 단위 컴포넌트
│   │   └── components/    ← 재사용 컴포넌트
│   ├── application/       ← 앱 로직 레이어
│   │   └── viewModels/    ← Zustand store + 비즈니스 로직
│   ├── domain/            ← 도메인 레이어
│   │   ├── entities/      ← 타입 정의
│   │   └── services/      ← 핵심 비즈니스 서비스
│   ├── data/              ← 데이터 레이어
│   │   └── repositories/  ← Firebase 연동
│   └── constants/         ← 상수 (사기 키워드 등)
├── planning/              ← 기획 문서
│   ├── decisions/         ← ADR (아키텍처 결정 기록)
│   └── ...
├── docs/                  ← 기술 문서
└── lessons/               ← 디버깅 사례
```

---

## 7. ADR 요약 (질의응답 준비)

### ADR-0001: React Native + Expo 선택
- **결정**: Flutter/Web 대신 React Native(Expo) 채택
- **이유**: 팀 JS 숙련도, 6주 내 Android 배포 목표, Expo Managed Workflow로 환경 복잡도 최소화
- **트레이드오프**: 네이티브 성능 일부 포기 → 개발속도로 보완

### ADR-0002: Zustand 상태관리
- **결정**: Redux 대신 Zustand 채택
- **이유**: 보일러플레이트 90% 감소, 학습 곡선 낮음, Firebase onSnapshot과 통합 용이
- **트레이드오프**: 대규모 앱에서 구조화 어려움 → 레이어드 아키텍처로 보완

### ADR-0003: Firebase 백엔드
- **결정**: 자체 서버 대신 Firebase(Firestore + Auth + Storage) 채택
- **이유**: 실시간 채팅 구현, 인증 시스템, 6주 일정 내 서버 구축 불가
- **트레이드오프**: 벤더 의존성 → 추후 마이그레이션 경로 설계

---

## 8. 개발 환경 설정 (Agent 실행 시 자동 참조)

```bash
# 필수 설치
node --version  # v18+
npm install -g expo-cli eas-cli

# 프로젝트 설치
git clone https://github.com/[username]/danggeun-nara
cd danggeun-nara
npm install

# 환경 변수 설정
cp .env.example .env
# .env 에 Firebase 키 입력

# 실행
npx expo start          # 개발 서버
npx expo start --android # Android 에뮬레이터
```

---

## 9. 빌드 & 배포 (Agent 참조용)

```bash
# 개발 빌드
eas build --platform android --profile development

# 프로덕션 APK
eas build --platform android --profile production

# 배포 확인
eas build:list
```

---

## 10. 암묵지 관리 (LLM Wiki 기반)

이 섹션은 개발 중 발견한 **암묵적 지식**을 LLM이 활용할 수 있도록 기록합니다.

### 10.1 Firestore 리스너 메모리 누수
- **상황**: 채팅방 반복 진입 시 메모리 누적
- **원인**: useEffect cleanup에서 onSnapshot 구독 해제 누락
- **해결**: `return () => unsubscribe()` 패턴 적용
- **참조**: lessons/firestore-listener-cleanup.md

### 10.2 Expo EAS Build 주의사항
- Android SDK 버전은 eas.json의 `buildType: "apk"` 설정 필수
- 환경변수는 `.env` 가 아닌 `eas secret:create` 로 등록
- 첫 빌드는 20~30분 소요 (캐시 없음)

### 10.3 안전점수 계산 최적화
- 상품 목록 렌더링 시 매번 calcSafeScore() 호출은 성능 저하
- 상품 저장 시 safeScore를 미리 계산해서 저장하는 패턴 적용
- 신고/후기 변경 시에만 재계산

---

*이 파일은 AI Agent(Claude, Cursor, Copilot 등)가 프로젝트를 즉시 이해하고  
올바른 방향으로 코드를 생성/수정할 수 있도록 설계된 통합 정책 파일입니다.*
