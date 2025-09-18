
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