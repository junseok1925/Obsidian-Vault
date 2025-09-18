
비동기란...?

작업이 완료될 때까지 기다리지 않고, 나중에 결과가 준비되면 그때 처리하는 방식

ex) 카페에서 주문을 할 때

|방식|설명|
|---|---|
|**동기**|주문 후 커피 나올 때까지 가만히 기다림|
|**비동기**|주문 후 진동벨 받고 → 자리에서 책 읽다가 → 진동벨 울리면 커피 받음|
Dart에서는 Future가 이 진동벨과 같은 역할을 함
- 플러터는 초당 60~120회 정도 화면을 그림
- 파일 다운로드, 웹 요청, 파일 저장 및 불러오기 작업 등등 시간이 오래 걸림
- 기다리지말고 다른 작업을 먼저 할 수 있더록 비동기를 사용한다.

---

### Future
- 2초 후 문자열을 리턴 함
- 실행은 즉시 시작되지만, 결과는 미래에 제공 됨

```dart
// 2초 후에 문자열을 반환하는 Future 함수
Future<String> fetchData() async {
  // 2초 기다렸다가 실행
  await Future.delayed(Duration(seconds: 2));
  return "데이터 로딩 완료!";
}

void main() async {
  print("데이터 가져오는 중...");

  // Future 결과를 기다림 (await 사용)
  String result = await fetchData();
  print(result);

  print("프로그램 종료");
}

// 실행결과
// 데이터 가져오는 중...
// (2초 대기)
// 데이터 로딩 완료!
// 프로그램 종료

```

then()을 사용하는 방법
```dart
Future<String> getMessage() {
  return Future.delayed(
    Duration(seconds: 1),
    () => "Hello from the Future!",
  );
}

void main() {
  print("시작");

  getMessage().then((msg) {
    print(msg); // 1초 뒤 실행
  });

  print("끝");
}

// 실행결과
// 시작
// 끝
// Hello from the Future!
```

---

### async/await

- await는 결과가 올 때까지 기다림
- async를 붙여야 await 사용 가능

```dart
// 2초 기다린 후 데이터를 반환하는 비동기 함수
Future<String> fetchData() async {
  await Future.delayed(Duration(seconds: 2)); // 2초 대기
  return "서버에서 가져온 데이터";
}

void main() async {
  print("데이터 가져오는 중...");

  // await를 쓰면 Future가 끝날 때까지 기다렸다가 result에 저장됨
  String result = await fetchData();
  print(result);

  print("프로그램 종료");
}

// 실행결과
// 데이터 가져오는 중...
// (2초 대기)
// 서버에서 가져온 데이터
// 프로그램 종료

```

---

### Nullable

아직 값이 없다는 뜻의 툭별한 값
Dart에서는 null 안전모드(null-safety)가 기본이라서, 명시적으로 nullable로 선언하지 않으면 null을 넣을 수 없다.

```dart

String name1 = null; : //이렇게 하면 오류남

String name2 = "강준석"; //이렇게 해도 오류남 ( 기본적으로 non-nullable) 이라서
name = null;
```

nullable 선언 방법

`String? name = null;` : ? (물음표) 붙이기


nullable 변수는 그냥 사용 불가
- nullable은 해당 변수가 null이라고 하는 것이 아니라 "null일 수도 있으니 조심하라" 라는 것
```dart
String? name2 = null
print(name.length);
// 에러남 null일 수도 있어서 바로 못 씀


String? name2 = null

if(name2 != null){
	print(name.lenght);
}
// 조건문으로 null 인지 체크해야지 사용가능
```

null 처리 방법들

- null 체크 먼저 해
```dart
String? name = null;
if (name != null) {
  print(name.length); // ✅ 안전
}
```

-  null-aware operator (`?.`)
```dart
String? name = null;
print(name?.length);  // name이 null이면 null 반환, 아니면 length
```

- default 값 주기 (`??`)
```dart
String? name = null;
print(name ?? "이름 없음"); // ??는 앞의 값이 null이면 뒤의 값으로 대체해줌
```

- null이 아님 명시 연산자 (`!`)
```dart
String? name = getName();
print(name!.length); // "절대 null 아님!"이라고 강제로 처리

String getName(){
	return "LEE";
}
```


----

### 예외처리

try/catch

```dart
void main() {
  try {
	  int? value = int.tryParse("가"); 
	  print("변환결과");
	  print(value!);
  } catch (e) {
	  // e: 실제로 발생한 예외 객체
    print("예외 발생: $e");
  }
}
```


finally
```dart
  try {
    print("시도 중...");
	  int? value = int.tryParse("가"); 
	  print("변환결과");
	  print(value!);
  } catch (e) {
    print("에러 발생!");
  } finally {
    print("마무리 실행됨");
  }
```

---

### enum

정해진 값들 중 하나만 선택할 수 있게 해주는 자료형
요일, 상태, 색깔 등 한정된 선택지를 가질 때 유용하게 사용

- enum을 사용하지 않았을 때
```dart
void printTodo(String day) {
  if (day == "월요일") {
    print('🧹 청소하기');
  } else if (day == "화요일") {
    print('🛍️ 장보기');
  } else if (day == "수요일") {
    print('🧼 빨래하기');
  } else if (day == "목요일") {
    print('🧾 장부 정리');
  } else if (day == "금요일") {
    print('🧠 공부 마무리');
  } else if (day == "토요일") {
    print('🍕 친구 만나기');
  } else if (day == "일요일") {
    print('😴 푹 쉬기!');
  }
}

void main(){
	printTodo("월요일"); // 청소하기
	printTodo("수요알"); // 출력안됨 - 이미 월요일에서 출력이 되어 if문 종료
}
```

 - enum을 사용했을  때
```dart
enum Weekday { monday, tuesday, wednesday, thursday, friday, saturday, sunday }

void printTodo(Weekday day) {
  if (day == Weekday.monday) {
    print('🧹 청소하기');
  } else if (day == Weekday.tuesday) {
    print('🛍️ 장보기');
  } else if (day == Weekday.wednesday) {
    print('🧼 빨래하기');
  } else if (day == Weekday.thursday) {
    print('🧾 장부 정리');
  } else if (day == Weekday.friday) {
    print('🧠 공부 마무리');
  } else if (day == Weekday.saturday) {
    print('🍕 친구 만나기');
  } else if (day == Weekday.sunday) {
    print('😴 푹 쉬기!');
  }
}

void main() {
  printTodo(Weekday.monday);   // 출력: 청소하기
  printTodo(Weekday.wednesday); // 출력: 빨래하기
  // printTodo(Weekday.www); // enum 에 포함안된 값 입력 시 에러!
}
```