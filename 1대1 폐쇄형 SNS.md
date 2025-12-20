
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




네, 요청하신 대로 각 필드가 **우리 앱 서비스 내에서 구체적으로 어떤 기능적 역할(Role)**을 담당하는지 핵심 위주로 정리해 드립니다.

이 내용을 알고 계시면 나중에 Flutter 화면을 그리거나 서버 로직을 짤 때 "아, 이 데이터를 여기서 쓰려고 만들었지!" 하고 바로 감이 오실 거예요.

---
네, 요청하신 대로 **데이터 타입(Type)**까지 포함하여 완벽하게 정리해 드립니다.

이 표는 나중에 Flutter에서 **클래스(Model)**를 설계하거나, Firestore **보안 규칙(Security Rules)**을 작성할 때 그대로 복사해서 사용하시면 됩니다.

---

### 1. `users` 컬렉션 (사용자 계정 관리)

각 유저의 고유 정보와 서비스 상태를 정의합니다.

|**필드명**|**데이터 유형 (Type)**|**역할 (Role)**|**활용 예시**|
|---|---|---|---|
|**`uid`**|**String**|고유 식별자|유저 고유 ID (Auth UID)|
|**`nickname`**|**String**|표시 이름|앱 내 노출 닉네임|
|**`email`**|**String**|계정 주소|로그인 및 식별용 이메일|
|**`activeRoomId`**|**String**|방 진입 키|소속된 방의 ID (Null이면 미연결)|
|**`pairCode`**|**String**|초대 코드|상대방 연결용 6자리 코드|
|**`provider`**|**String**|가입 경로|`google`, `email` 등 가입 수단|
|**`status`**|**String**|계정 상태|`active`, `pending`, `deleted`|
|**`fcmToken`**|**String**|푸시 주소|알림 전송용 토큰|
|**`pushEnabled`**|**Boolean**|알림 설정|알림 수신 동의 여부 (`true`/`false`)|
|**`os`**|**String**|기기 환경|`ios`, `android` 구분|
|**`createdAt`**|**Timestamp**|생성 시점|계정 생성 일시|

---

### 2. `rooms` 컬렉션 (커플 공유 공간)

위젯 화면에서 가장 먼저 읽어오는 '대표 데이터'입니다.

|**필드명**|**데이터 유형 (Type)**|**역할 (Role)**|**활용 예시**|
|---|---|---|---|
|**`member`**|**Array (String)**|출입 명부|`["uidA", "uidB"]` 권한 확인|
|**`startDate`**|**Timestamp**|기념일 기준|사귀기 시작한 날 (D-Day 계산)|
|**`lastPhotoUrl`**|**String**|위젯 이미지|위젯에 표시할 최신 사진 URL|
|**`lastWidgetText`**|**String**|위젯 문구|위젯에 표시할 메인 멘트|
|**`lastPhotoAt`**|**Timestamp**|시간 정보|마지막 사진이 올라온 시각|
|**`createdAt`**|**Timestamp**|생성 시점|방이 개설된 일시|

---

### 3. `photos` 하위 컬렉션 (사진 및 AI 분석 상세)

모든 사진 히스토리와 AI가 가공한 데이터를 담고 있습니다.

> **경로:** `/rooms/{roomId}/photos/{photoId}`

|**필드명**|**데이터 유형 (Type)**|**역할 (Role)**|**활용 예시**|
|---|---|---|---|
|**`senderId`**|**String**|작성자 식별|사진을 보낸 사람의 UID|
|**`imageUrl`**|**String**|이미지 주소|스토리지 다운로드 링크 (표시용)|
|**`storagePath`**|**String**|파일 경로|스토리지 내 실제 경로 (삭제/관리용)|
|**`emotionTag`**|**String**|감정 아이콘|선택한 감정 이미지 URL 또는 이름|
|**`mainColor`**|**String**|UI 테마 색상|`#FFFFFF` 등 헥사코드 (UI 자동 색상)|
|**`widgetText`**|**String**|사진 멘트|사진과 함께 기록된 짧은 문구|
|**`createdAt`**|**Timestamp**|생성 시점|사진 업로드 일시|
|**`visionAnalysis`**|**Map**|AI 분석 그룹|AI가 분석한 정보의 묶음|
|└ **`label`**|**Array (String)**|객체 인식 결과|`["sea", "night", "outside"]`|