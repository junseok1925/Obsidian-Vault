# 최종 SA  
---

# 1. 프로젝트 개요 — 큐티 스토리♥️

- TikTok 스타일의 이미지/숏폼 피드 SNS
    
- 모든 게시물은 세로 스와이프(PageView)로 감상
    
- FirebaseAuth 로그인/회원가입 적용
    
- 게시글/댓글/좋아요 모두 로그인한 사용자만 가능
    
- 최신 게시물 상단 노출
    
- 무한 스크롤 지원
    

---

# 2. 요구사항 정의

---

## 2.1 기능 요구사항

### 1) 피드(홈)

- 최신순 게시물 조회 (createdAt DESC)
    
- 이미지 또는 영상(숏폼)
    
- PageView.builder 기반 위/아래 스와이프
    
- 게시물 정보:
    
    - 닉네임, 이미지/영상, 내용
        
    - 작성 시간
        
    - 좋아요 수
        
    - 댓글 수
        
    - 수정 버튼(authorId == currentUser.uid)
        
- 무한스크롤
    

---

### 2) 게시물 작성 (UploadPage)

- 이미지/영상 선택
    
- 텍스트 입력
    
- “업로드” → Storage 업로드 후 Firestore 저장
    
- `authorId = FirebaseAuth.currentUser.uid`
    
- `nickname` 은 users 컬렉션에서 가져와 저장
    

---

### 3) 게시물 수정

- authorId == 현재 로그인 유저일 때만 가능
    
- 텍스트 수정
    
- 이미지/영상 다시 업로드 가능
    
- Firestore update
    

---

### 4) 댓글 기능

- 로그인 사용자만 작성 가능
    
- 댓글: 내용 + 작성 시간 + 닉네임 + authorId
    
- 댓글 수 실시간 반영
    

---

### 5) 마이페이지

- 내가 작성한 게시글 목록 조회
    
- 게시글 클릭 → 댓글 화면 이동 또는 수정 화면 이동
    

---

### 6) 인증 (Auth)

- Firebase Auth (이메일/비밀번호)
    
- 닉네임은 별도 users 컬렉션에 저장
    
- 로그인/로그아웃/회원가입 화면 구성
    

---

## 2.2 비기능 요구사항

- 안정적인 무한 스크롤
    
- Firebase Storage 업로드 시 진행상태 표시
    
- 예외 처리 (네트워크 실패 등)
    
- Crashlytics 적용
    

---

# 3. 데이터 모델 (Firebase 구조)

---

## 3.1 users

|필드|타입|설명|
|---|---|---|
|uid|string|Firebase Auth UID|
|nickname|string|사용자 닉네임|
|photoUrl|string?|프로필 이미지|

---

## 3.2 posts

|필드|타입|설명|
|---|---|---|
|id|string|문서 ID|
|mediaUrl|string|Storage 경로|
|mediaType|string|image / video|
|content|string?|게시글 텍스트|
|createdAt|Timestamp|생성 시간|
|authorId|string|Firebase UID|
|nickname|string|작성자 닉네임|
|likeCount|int|좋아요 수|
|commentCount|int|댓글 수|

---

## 3.3 comments

|필드|타입|설명|
|---|---|---|
|id|string||
|postId|string||
|content|string||
|createdAt|Timestamp||
|authorId|string||
|nickname|string||

---

# 4. 전체 아키텍처

---

## 4.1 Layer 구조 (Clean Architecture)

`lib/   app/   core/   features/     auth/     feed/     upload/     comments/     profile/`

---

## 4.2 Presentation Layer (UI)

- FeedPage
    
- UploadPage
    
- CommentPage
    
- EditPostPage
    
- LoginPage
    
- SignupPage
    
- MyPage
    

---

## 4.3 Domain Layer

- Entities (User, Post, Comment)
    
- Repository Interfaces
    

예) PostRepository

`abstract class PostRepository {   Stream<List<Post>> watchFeed({required int limit});   Future<List<Post>> fetchMore(Post? lastPost, int limit);   Future<void> createPost(Post post, File mediaFile);   Future<void> updatePost(Post post, {File? newMediaFile});   Future<void> likePost(String postId, String userId); }`

---

## 4.4 Data Layer

- Firebase Datasource
    
- Model ↔ Entity 변환
    
- Firestore + Storage 연결
    
- RepositoryImpl
    

---

# 5. 동작 플로우

---

## 5.1 로그인/회원가입

### 회원가입

1. email/password 입력
    
2. FirebaseAuth.createUserWithEmailAndPassword
    
3. users 컬렉션에 nickname 저장
    
4. 로그인 상태로 홈 이동
    

### 로그인

1. FirebaseAuth.signInWithEmailAndPassword
    
2. 유저 정보 로딩
    
3. FeedPage로 이동
    

---

## 5.2 게시물 업로드

1. 이미지/영상 선택
    
2. 텍스트 입력
    
3. Firebase Storage 업로드
    
4. Storage URL 획득
    
5. Firestore posts 컬렉션 생성
    

`{   "mediaUrl": "...",   "mediaType": "image",   "content": "...",   "createdAt": now,   "authorId": uid,   "nickname": "홍길동",   "likeCount": 0,   "commentCount": 0 }`

6. 피드 자동 업데이트
    

---

## 5.3 피드 무한 스크롤

`postsRef   .orderBy("createdAt", descending: true)   .limit(10)`

다음 페이징:

`.startAfter(lastDoc["createdAt"])`

---

## 5.4 게시물 수정

1. authorId == currentUser.uid 체크
    
2. 이미지 변경 시 Storage 재업로드
    
3. Firestore update
    

---

## 5.5 댓글 기능

1. 댓글 입력 → comments 컬렉션 insert
    
2. posts.commentCount 증가
    
3. 댓글 리스트 실시간 표시
    

---

# 6. Firestore Security Rules (필수)

`rules_version = '2'; service cloud.firestore {   match /databases/{database}/documents {      match /users/{uid} {       allow read: if true;       allow create, update: if request.auth != null                              && uid == request.auth.uid;     }      match /posts/{postId} {       allow read: if true;        allow create: if request.auth != null;        allow update, delete: if request.auth != null              && request.auth.uid == resource.data.authorId;     }      match /comments/{commentId} {       allow read: if true;        allow create: if request.auth != null;        allow update, delete: if request.auth != null             && request.auth.uid == resource.data.authorId;     }   } }`

---

# 7. 기술 스택 정리

|영역|기술|
|---|---|
|Frontend|Flutter, Riverpod, Freezed|
|Backend|Firebase Auth, Firestore, Firebase Storage|
|Logging|Crashlytics|
|Media|image_picker, video_player|
|Architecture|Clean Architecture + Feature Layered|

---

# 🔥 이제 **프로젝트 틀을 어떻게 시작해야 하는가? (실행 순서)**

아래 순서 그대로 실행하면 실패 없이 프로젝트가 잡힌다.

---

# 1단계 — Flutter + Firebase 기본 세팅

- flutter create
    
- firebase_core 세팅
    
- firebase_auth 세팅
    
- firestore, storage 연결 확인
    

---

# 2단계 — 폴더구조 먼저 만들기 (Clean Architecture 뼈대)

폴더만 만들어 놓기:

`features/auth/ features/feed/ features/upload/ features/comments/ features/profile/`

---

# 3단계 — Domain 레이어 설계

각 feature마다:

- Entity (Post, Comment, UserProfile)
    
- Repository Interface
    

**아직 구현 NO → “틀”만 먼저 만든다.**

---

# 4단계 — Data 레이어 구현

- Model (PostModel, CommentModel)
    
- Firebase Datasource
    
- RepositoryImpl (CRUD 구현)
    

---

# 5단계 — Presentation 기본 Provider 만들기

예:

`final feedStreamProvider = StreamProvider<List<Post>>((ref) {   final repo = ref.watch(postRepositoryProvider);   return repo.watchFeed(limit: 10); });`

---

# 6단계 — UI 생성

가장 먼저 FeedPage를 만든다.

- PageView.builder
    
- 각 Post 렌더링
    
- 댓글/좋아요 UI 배치
    

그 후 UploadPage → CommentPage → MyPage 순서.

---

# 7단계 — 수정 기능(Author 체크) 추가

`if (post.authorId == FirebaseAuth.instance.currentUser!.uid)     수정 버튼 표시`

---

# 8단계 — 전체 연결 및 테스트

---

# 원하면 다음 단계로 만들어줄 수 있음

- FeedPage의 실제 코드 구조
    
- UploadPage 전체 로직 (이미지 선택 → Storage 업로드 → Firestore 저장)
    
- 댓글 페이지 전체 코드
    
- Auth UI + Provider
    
- RepositoryImpl 상세 로직
    
- 완성된 초기 프로젝트 템플릿 만들기
    

계속 진행할까? 어떤 부분부터 구현을 시작할지 말해줘.