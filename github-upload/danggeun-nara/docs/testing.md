# 테스트 가이드 — 당근나라 PC

---

## 테스트 실행

```bash
# 전체 테스트
npm test

# 커버리지 포함
npm test -- --coverage

# 특정 파일만
npm test -- ProductRepository.test.ts

# 감시 모드
npm test -- --watch
```

---

## 테스트 구조 (70 / 20 / 10 비율)

```
src/
└── __tests__/
    ├── unit/               # 단위 테스트 (70%)
    │   ├── PriceValidator.test.ts
    │   ├── ProductRepository.test.ts
    │   ├── useProductList.test.ts
    │   ├── useAuth.test.ts
    │   └── ChatRepository.test.ts
    ├── component/          # 컴포넌트 테스트 (20%)
    │   ├── ProductCard.test.tsx
    │   └── CategoryFilter.test.tsx
    └── integration/        # 통합 테스트 (10%)
        └── ProductFlow.test.ts
```

---

## 단위 테스트 5개

### 1. PriceValidator — 가격 유효성 검사

```typescript
// should_return_false_when_price_is_zero
test('should_return_false_when_price_is_zero', () => {
  expect(PriceValidator.isValid(0)).toBe(false);
});

// should_return_false_when_price_is_negative
test('should_return_false_when_price_is_negative', () => {
  expect(PriceValidator.isValid(-1000)).toBe(false);
});

// should_return_true_when_price_is_valid
test('should_return_true_when_price_is_valid', () => {
  expect(PriceValidator.isValid(50000)).toBe(true);
});

// should_return_false_when_price_exceeds_max
test('should_return_false_when_price_exceeds_max', () => {
  expect(PriceValidator.isValid(99999999 + 1)).toBe(false);
});

// should_return_false_when_price_is_null
test('should_return_false_when_price_is_null', () => {
  expect(PriceValidator.isValid(null)).toBe(false);
});
```

### 2. ProductRepository — 상품 목록 조회

```typescript
// should_return_empty_array_when_no_products
test('should_return_empty_array_when_no_products', async () => {
  mockFirestore.getDocs.mockResolvedValue({ docs: [] });
  const result = await ProductRepository.getAll();
  expect(result).toEqual([]);
});

// should_return_products_filtered_by_category
test('should_return_products_filtered_by_category', async () => {
  const result = await ProductRepository.getByCategory('마우스');
  expect(result.every(p => p.category === '마우스')).toBe(true);
});
```

### 3. useAuth — 로그인 상태 관리

```typescript
// should_set_user_when_login_succeeds
test('should_set_user_when_login_succeeds', async () => {
  const { result } = renderHook(() => useAuth());
  await act(async () => { await result.current.login('test@test.com', '1234'); });
  expect(result.current.user).not.toBeNull();
});
```

---

## 통합 테스트 1개

```typescript
// 상품 등록 → 목록 조회 → 상세 확인 시나리오
test('should_show_new_product_in_list_after_registration', async () => {
  // 1. 상품 등록
  await ProductRepository.create({
    title: 'RTX 4080 테스트',
    price: 850000,
    category: '그래픽카드',
  });

  // 2. 목록 조회
  const products = await ProductRepository.getAll();

  // 3. 등록한 상품 확인
  const found = products.find(p => p.title === 'RTX 4080 테스트');
  expect(found).toBeDefined();
  expect(found?.price).toBe(850000);
});
```

---

## AI 사용 고지

테스트 코드 초안은 AI Agent가 생성했으며, 다음을 직접 검토·수정했습니다:
- edge case 누락 여부 확인
- mock 설정 오류 수정
- 테스트 이름 컨벤션 통일 (`should_~_when_~`)

---

## 커버리지 목표

| 영역 | 목표 | 현재 |
|------|------|------|
| domain/ | 80% | 75% |
| application/ | 70% | 60% |
| data/ | 60% | 50% |
