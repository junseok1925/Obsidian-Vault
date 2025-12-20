
---

# 서버 담당자 작업 범위 (Firebase 기준)


---

## 1️⃣ Firebase 프로젝트 세팅

- Firebase 프로젝트 생성
    
- iOS 앱 등록
    
- GoogleService-Info.plist 발급
    
- dev / prod 분리 (권장)
    

---

## 2️⃣ Authentication 설정

### 사용

- Apple 로그인
    
- Google 로그인
    

### 해야 할 일

- Firebase Auth Provider 설정
    
- 로그인 성공 시 user 문서 생성 로직 정의
    

👉 결과물: **로그인하면 users 문서 자동 생성**

---

## 3️⃣ Firestore DB 설계 & 생성

### 컬렉션 구조 (확정)

`users/{uid} rooms/{roomId} photos/{photoId}`

### users

- nickname
    
- profileImg
    
- pairCode ⭐
    
- activeRoomId
    
- createdAt
    

### rooms

- members: [uidA, uidB]
    
- startDate ⭐
    
- lastPhotoUrl
    
- lastPhotoAt
    
- createdAt
    

### photos

- roomId
    
- senderId
    
- imageUrl
    
- emotionTag
    
- widgetText
    
- createdAt
    

---

## 4️⃣ 페어링 로직 (Cloud Functions)

### 네가 직접 구현해야 할 핵심

**Cloud Function: pairWithCode**

- 상대 `pairCode` 검증
    
- 자기 자신 코드 차단
    
- 이미 페어링된 유저 차단
    
- room 생성
    
- users.activeRoomId 업데이트 (양쪽)
    
- pairCode 재사용 여부 결정
    

👉 결과물: **안전한 페어링 서버 로직**

---

## 5️⃣ Firebase Storage 설계

- photos 폴더 구성
    
- Private 접근
    
- 업로드 파일 크기 제한
    
- 다운로드 URL 생성 방식 정의
    

👉 결과물: **사진 저장 규칙**

---

## 6️⃣ Security Rules (중요)

### Firestore Rules

- users: 본인만 접근
    
- rooms: members 포함된 유저만 접근
    
- photos: roomId 기준 접근
    

### Storage Rules

- 로그인 유저만 업로드
    
- 본인 room 사진만 접근
    

👉 결과물: **보안 규칙 완성**

---

## 7️⃣ 실시간 동기화 설계

- rooms 문서 실시간 리스닝
    
- lastPhotoUrl / lastPhotoAt 변경 감지
    
- 위젯 갱신 트리거 정의
    

👉 결과물: **앱 + 위젯 실시간 갱신 가능**

---

## 8️⃣ AI 분석 파이프라인 (Firebase 기준)

### Cloud Function: analyzePhoto

`사진 업로드  → Storage 트리거  → Google Vision AI 호출  → 분석 결과 JSON  → 상황 키워드 추출  → widgetText 생성  → photos / rooms 업데이트`

- Vision API 키 서버에서 관리
    
- 이미지 클라이언트 직접 전달 ❌
    

👉 결과물: **사진 → 문구 자동 생성**

---

## 9️⃣ (선택) ChatGPT 연동

- Vision 요약 텍스트만 전달
    
- 짧은 문구 1~2개 생성
    
- 실패 시 룰 기반 fallback
    

👉 결과물: **문구 품질 개선**

---

## 🔟 D-Day / 기념일 처리

### 서버 책임

- rooms.startDate 저장
    
- (선택) 기념일 당일 판별 함수 제공
    

`days = today - startDate + 1`

👉 계산은 프론트 or 서버 중 하나로 통일

---

## 1️⃣1️⃣ 에러 케이스 정의

- 잘못된 pairCode
    
- 이미 페어링된 유저
    
- Vision API 실패
    
- Storage 업로드 실패
    
- 권한 오류
    

👉 결과물: **에러 코드 규약**

---

## 1️⃣2️⃣ 문서화

- Firestore 구조 설명
    
- Cloud Functions 목록
    
- 요청/응답 예시
    
- 주의사항
    

👉 결과물: **README / Notion**

---

## 절대 하지 말 것

- Vision API를 클라이언트에서 호출
    
- pairCode를 rooms에 저장
    
- Security Rules 없이 테스트 진행
    
- 이미지 원본 로그 저장
    

---

## 네 파트 완료 기준

- Firebase 프로젝트
    
- Auth 정상
    
- Firestore + Rules
    
- Storage + Rules
    
- 페어링 Function
    
- AI 분석 Function
    
- 문서
    

---

## 한 줄 결론

**“Firebase에서는 서버가 보이지 않아도,  
서버 역할은 반드시 누군가가 해야 한다.”**

지금 이 구조면  
👉 iOS 단일 출시  
👉 MVP  
👉 이후 확장  
전부 문제 없다.




### 2. 컬렉션 구조 및 필드 상세 설계

Firestore는 `컬렉션(폴더) > 문서(파일) > 필드(데이터)` 구조입니다. 아래 3개의 핵심 컬렉션을 만드세요.

#### **① `users` 컬렉션**

각 사용자의 개인 정보와 상태를 저장합니다.

- **문서 ID:** 사용자의 `uid` (Firebase Auth에서 발급되는 고유 ID)
    
- **필드:**
    
    - `nickname`: string (이름)
        
    - `pairCode`: string (페어링용 고유 코드)
        
    - `activeRoomId`: string (현재 연결된 방 ID, 없으면 null)
        
    - `fcmToken`: string (알림 전송용 토큰)
        
    - `createdAt`: timestamp
        

#### **② `rooms` 컬렉션**

커플 2명이 공유하는 공간입니다. **위젯은 이 문서를 실시간으로 관찰**하게 됩니다.

- **문서 ID:** 자동 생성 (Auto-ID)
    
- **필드:**
    
    - `members`: array (유저 A의 uid, 유저 B의 uid 저장)
        
    - `startDate`: timestamp (사귀기 시작한 날)
        
    - `lastPhotoUrl`: string (가장 최근 업로드된 사진 경로)
        
    - `lastWidgetText`: string (AI가 생성한 추천 문구)
        
    - `lastPhotoAt`: timestamp (마지막 업데이트 시각)
        

#### **③ `photos` 컬렉션**

전체 사진 기록(타임라인)을 저장합니다.

- **문서 ID:** 자동 생성 (Auto-ID)
    
- **필드:**
    
    - `roomId`: string (어느 커플의 사진인지 구분)
        
    - `senderId`: string (보낸 사람 uid)
        
    - `imageUrl`: string (Storage 저장 경로)
        
    - `visionLabels`: array (Vision AI가 분석한 키워드들)
        
    - `widgetText`: string (이 사진 전용 AI 문구)
        
    - `createdAt`: timestamp
        

---

### 3. 백엔드 담당자의 핵심 팁 (중요)

**1) 데이터 중복 저장(Denormalization)의 이유:** 위젯은 성능이 중요합니다. 위젯이 `photos` 컬렉션에서 "이 방의 가장 최신 사진이 뭐지?"라고 쿼리(검색)하게 만들면 비용도 많이 들고 느립니다. 그래서 **`rooms` 문서에 최신 사진 정보를 항상 복사해두는 것**입니다.

**2) Index(색인) 설정:** 나중에 타임라인 기능을 만들 때 "특정 방의 사진을 시간 역순으로 불러오기"를 하려면 Firestore에서 **복합 색인(Composite Index)** 설정이 필요합니다. 이건 나중에 Flutter에서 쿼리를 날리면 에러 메시지에 링크가 뜨는데, 그걸 클릭만 하면 자동으로 만들어집니다.

**3) Storage 경로 규칙:** 사진 파일은 Firebase Storage에 저장할 때 `/photos/{roomId}/{photoId}.jpg` 식으로 방 ID별로 폴더를 나누어 저장하는 것이 보안과 관리에 좋습니다.

---

### 4. 다음 단계 제안

컬렉션을 만드셨다면, 이제 **"내 페어링 코드를 생성하고 상대방 코드를 입력하면 `rooms` 문서를 만들어주는 Cloud Function"** 코드를 짤 차례입니다.

**페어링 로직(TypeScript 코드)을 먼저 짜드릴까요, 아니면 Flutter 앱에서 데이터를 넣는 법부터 알아볼까요?**





OS"ios_test_보류"

(string)

activeRoomId"ZiarlTTbW7i47V0GAhQI"

(string)

createdAt2025년 12월 21일 AM 1시 27분 10초 UTC+9

(timestamp)

fcmToken"token_test_123"

(string)

nickname"태유니엉뜨"

(string)

pairCode"A1b2c3"