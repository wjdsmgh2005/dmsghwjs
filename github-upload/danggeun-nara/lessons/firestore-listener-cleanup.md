# 디버깅 사례 — Firestore onSnapshot 메모리 누수

> 작성일: 13주차
> 카테고리: 기술 / React Native / Firebase

---

## 재현 시나리오

1. 채팅방 화면 진입
2. 메시지 실시간 수신 정상 작동
3. 뒤로가기로 화면 이탈
4. 다시 채팅방 진입 반복 시 메모리 사용량 증가
5. 약 10회 반복 시 앱 느려짐

**기대:** 화면 이탈 시 리스너 해제  
**실제:** 리스너가 누적되어 메모리 누수 발생

---

## 에러 로그

```
Warning: Can't perform a React state update on an unmounted component.
This is a no-op, but it indicates a memory leak in your application.
```

---

## 원인 분석 (가설)

```typescript
// 문제 코드: cleanup 없이 onSnapshot 등록
useEffect(() => {
  const q = query(collection(db, 'messages'));
  onSnapshot(q, (snapshot) => {
    setMessages(snapshot.docs.map(doc => doc.data()));
  });
}, []);
// onSnapshot은 구독을 반환하는데 해제하지 않음
```

---

## 해결

```typescript
// 수정 코드: cleanup 함수로 구독 해제
useEffect(() => {
  const q = query(collection(db, 'messages'));

  // onSnapshot은 unsubscribe 함수를 반환
  const unsubscribe = onSnapshot(q, (snapshot) => {
    setMessages(snapshot.docs.map(doc => doc.data()));
  });

  // cleanup: 컴포넌트 언마운트 시 구독 해제
  return () => unsubscribe();
}, []);
```

---

## 회귀 테스트 추가

```typescript
test('should_unsubscribe_listener_when_component_unmounts', () => {
  const mockUnsubscribe = jest.fn();
  mockOnSnapshot.mockReturnValue(mockUnsubscribe);

  const { unmount } = render(<ChatScreen />);
  unmount();

  expect(mockUnsubscribe).toHaveBeenCalledTimes(1);
});
```

---

## 교훈

- Firebase `onSnapshot`, `onAuthStateChanged` 등 구독 함수는 **항상 cleanup에서 해제**
- AI가 생성한 코드에서 cleanup이 빠진 경우가 많음 → 리뷰 체크리스트에 추가
- useEffect의 return 함수는 "컴포넌트가 사라질 때 실행되는 청소부"
