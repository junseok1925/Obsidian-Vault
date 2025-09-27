
만약 오류가 발생했을 때 예외를 처리해 주지 않으면 프로그램이 종료
처리해 주면 프로그램이 정상적으로 실행된다.

모든 종류의 예외는 `Exception`클래스를 상속 받아서 구성된다. (모든 예외는 `Exception` 클래스 하위 클래스)

`Exception` 클래스를 상속 받지 않고 `Exception` 클래스로 객체를 생성하면
어떤 타입의 예외인지 알기 어렵기 때문에 `Exception` 클래스를 상속 받은 클래스로 객체를 생성해주는 것이 좋다.
```dart
var exception = Exception('예외처리된 ')
```

## Dart의 기본 예외 & 에러 정리

| 종류                 | 클래스명                               | 발생 상황          | 예제 코드                            |
| ------------------ | ---------------------------------- | -------------- | -------------------------------- |
| **예외 (Exception)** | **FormatException**                | 잘못된 형식 변환      | `int.parse("abc");`              |
|                    | **IntegerDivisionByZeroException** | 정수를 0으로 나눌 때   | `10 ~/ 0;`                       |
|                    | **IOException** (`dart:io`)        | 파일/네트워크 입출력 오류 | `File("x.txt").readAsString();`  |
|                    | **UnsupportedError**               | 지원하지 않는 기능 사용  | `list.length = -1;`              |
|                    | **StateError**                     | 잘못된 상태에서 호출    | `[].first;`                      |
|                    | **ArgumentError**                  | 잘못된 인자 전달      | `throw ArgumentError("잘못된 인자");` |
| **에러 (Error)**     | **NoSuchMethodError**              | 없는 메서드 호출      | `null.toStringAsFixed(2);`       |
|                    | **TypeError**                      | 잘못된 타입 사용      | `int x = "hi";`                  |
|                    | **RangeError**                     | 범위를 벗어난 접근     | `[1,2,3][5];`                    |
|                    | **AssertionError**                 | assert 조건 실패   | `assert(1 > 2);`                 |