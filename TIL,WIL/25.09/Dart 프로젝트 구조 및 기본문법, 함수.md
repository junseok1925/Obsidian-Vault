- vs코드 커멘트 팔레트에 `dart new project` 입력
- `Console Application` 선택
- 프로젝트 생성 위치 및 이름 설정
- 

### Dart 기초 문법

- dart 함수 기초 무법 틀
```dart
void main() {
	// 이 부분에 dart 실행 코드를 적성해야함
}
```


- 산술 연산자
```dart
// 덧셈 +
print(100+1);
print("100" + "1"); // => 결과는?
// 뺄셈 -
print(100-1);
// 곱셈 *
print(3 * 5);
print("100" * 10); // => 결과는?
// 나눗셈 /
print(5 / 2); // 2.5
// 나머지 %
print(5 % 2); // 1
// 몫 ~/
print(5 ~/ 2); // 2
```

- 비교 연산자
```dart
print(5 == 5);     // true (같다)
print(5 != 3);     // true (같지 않다)
print(10 > 7);     // true (크다)
print(2 < 1);      // false (작다)
print(3 >= 3);     // true (크거나 같다)
print(4 <= 2);     // false (작거나 같다)
```

- 논리 연산자
	- `&&`: **그리고** — 둘 다 참일 때만 true
	- `||`: **또는** — 하나라도 참이면 true
	- `!`: **반전** — true는 false로, false는 true로
	
```dart
print(true && false);  // false (그리고)
print(true || false);  // true (또는)
print(!true);          // false (부정)
```

- 변수
	- 선언된 변수의 값을 가져올 수 있음
	
		**변수의 타입**
	- `double`: 소수
	- `String`: 문자열
	- `int`: 정수
	- `bool`: 참/거짓
	- 등등
```dart
const name = '강준석';
print("이름명 : ${name}")
```

```dart
double height = 174.5;
String name = "강준석";
int age = 29;
bool isOpen = true;
```

 - `var` 은 Dart에서 자동으로 타입을 추론해준다
 - 한 번 정해진 타입은 바뀌지 않는다
```dart
var pi = 3.14159;       // => double로 인식
var appName = "카카오톡"; // => String으로 인식
var price = 100;
// price = "만원"; ❌ 오류 발생! 처음에 int로 정해졌기 때문
```

---

### 함수

- 함수 정의방법

```dart

// void : 결과값 없이 실행만 한다는 뜻
// sayHello(): 함수 이름 (블록의 이름)
void sayHello() {
  print('안녕하세요!');
}

void printName(String name) {
	print("이름은 $name입니다");
}

void main() {
  // printName 함수를 실제로 호출
  printName("철수");
  printName("영희");
}

double circle(double r) {
	return 2 * 3.14159 * r;
}

void main(){
	double result = circle(5); 
	print(result);
	// 31.4159
}
```