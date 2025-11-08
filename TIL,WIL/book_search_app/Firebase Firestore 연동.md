
- 서버 구현없이 앱개발을 편하게 할 수 있게 도와주는 플랫폼

## 1. Firebase 프로젝트 생성

공식 홈페이지 접속 [https://firebase.google.com/](https://firebase.google.com/)

![[Pasted image 20251109014529.png]]

1. 새 firebase 프로젝트 만들기
2. 프로젝트 이름 지정

## 2. Firebase Database 생성
![[Pasted image 20251109014641.png]]

1. 생성된 프로젝트 들어간 후 왼쪽 메뉴 → 빌드 → Firestore Database

![[Pasted image 20251109014820.png]]

2. 컬렉션 시작을 통해 넣을 문서 값 초기 설정


## 3. Flutter에 Firebase 연동

```bash

1. 맥 터미널에 Firebase CLI 자동 설치 스크립트 입력
   
	curl -sL https://firebase.tools | bash

2. 로그인 스크립트 입력 후 나오는 브라우저에 구글 계정 로그인
   
	firebase login

3. VSCode 터미널로 가서 입력해서 프로젝트에 Firebase 자동 구성 시작
   자동 구성할 폴더 선택 후 사용 환경 설정 (안드로이드,ios,macOS...)
   
	flutterfire configure

3. firebase_core 패키지 추가
   
	flutter pub add firebase_core

4. os/Podfile 패일에서 IOS 최소 지원 버전 바꾸기(IOS 13 이상부터 flutter firebase_core 사용할 수 있음)
   
5. firebase_firestore 패키지 추가
   
	flutter pub add cloud_firestore
```


**main.dart 의 main 함수에서 Firebase 초기화 코드 추가**

```dart
import 'package:firebase_core/firebase_core.dart';
import 'package:flutter/material.dart';
import 'package:flutter_firebase_blog_app/firebase_options.dart';
import 'package:flutter_firebase_blog_app/ui/home/home_page.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';

void main() async {
  // 1.
  WidgetsFlutterBinding.ensureInitialized();

  await Firebase.initializeApp
  (options: DefaultFirebaseOptions.currentPlatform);
  // provider 전역 사용
  runApp(ProviderScope(child: MyApp()));
}
```

1. `WidgetsFlutterBinding.ensureInitialized();`
	   Flutter 프레임워크와 엔진을 초기화
	   `main()`에서 `runApp()` 전에 **비동기 코드(`await`)**를 실행할 경우 반드시 필요하다.
	   즉, Flutter의 엔진이 준비되기 전에 Firebase를 초기화하지 못하도록 보장한다.
	
 2. `await Firebase.initializeApp(options: DefaultFirebaseOptions.currentPlatform);`
	각 플랫폼(Android, iOS, Web 등)에 맞는 Firebase 설정이 자동으로 적용된다.

→ firebase 연동 완료