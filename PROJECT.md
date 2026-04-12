# 의류 매출 관리 — 프로젝트 메모

## 파일 위치
- `Cloth-Sales-Report/index.html` (서버 없이 브라우저로 바로 열기)

## 기능
- 월별 캘린더 뷰 (← → 화살표로 월 이동)
- 날짜 클릭 → 카드/현금 매출 입력
- 일 합계 실시간 자동 계산
- 주간합계 (오른쪽 열): 카드합계 / 현금합계 / 주간총합
- 월간 요약 (상단 바): 카드합계 / 현금합계 / 월총매출 / 영업일수
- 모바일 반응형 적용 완료 (주간합계 열 숨김, 월간요약 2x2)

## Firebase 연동 (완료)
- Firestore 실시간 동기화 — 여러 기기에서 같은 데이터 자동 공유
- 저장/삭제 → Firestore write/delete
- 연결 상태 표시 (우하단 뱃지)
- 기존 localStorage 데이터 → Firebase 마이그레이션 배너 (첫 연결 시 1회)
- Firebase 미설정 시 자동으로 localStorage 모드로 fallback

## Firebase 설정 방법
1. https://console.firebase.google.com 접속 → 프로젝트 생성
2. Firestore Database 생성 (테스트 모드로 시작)
3. 프로젝트 설정 → 내 앱 → 웹앱 추가 → SDK 설정 복사
4. index.html 상단 `firebaseConfig` 객체에 값 붙여넣기

## Firestore 보안 규칙 (개인용)
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /cloth-sales/{document=**} {
      allow read, write: if true;
    }
  }
}
```

## 데이터 형식 (Firestore & localStorage 동일)
```json
컬렉션: cloth-sales
문서 ID: "2026-04-06"
필드: { "card": 1200000, "cash": 350000 }
```

## 셀 표시
- 파란 점 🔵 = 카드
- 초록 점 🟢 = 현금
- 굵은 숫자 = 일 합계

## Sales-Report와의 차이점
- 색상 테마: 주황색 계열
- Firebase 컬렉션: `cloth-sales` (기존 `sales`와 완전 분리)
- localStorage 키: `cloth-sales-data` (기존 데이터와 충돌 없음)

## 나중에 하고 싶은 것
- 매출 페이지 비밀번호 잠금 (Firebase Auth 이용)
- 월별 비교 차트
