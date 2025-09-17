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
void main() {
  sayHello();
}
void sayHello() {
  print('Hi!!!!');
}

결과 : Hi!!!!

```

```dart
void printName(String name){
	print("이름은 $name입니다");
}

void main(){
	printName('강준석');
	printName('kangjunseok');
}

결과: 이름은 강준석입니다
	 이름은 kangjunesok입니다

```

```dart
double circle(double r){ //circle 함수는 매개변수 r를 받는다
	return 2 * 3.14159 * r;
}

void main(){
	double result = circle(5); // 매개변수 double r 에 5가 할당 된다.
	print(result);
}

결과 : 31.4159
```

위치 기반 파라미터
- 순서대로 값을 전달해야 하는 방식
```dart
void greet(String name, int age){
	print('이름: $name, 나이: $age');
}

void main(){
	greet('강준석', 29); //순서 지켜야 함
}
```

이름 있는 파라미터
 - 파라미터의 이름을 명시적으로 지정해서 전달하는 방식
 - `required`키워드로 꼭 필요한 값 지정 가능
```dart
void greet(required String name, int age){
	print('이름: $name, 나이: $age');
}

void main(){
	greet(name: '강준석', age: 29); // 파라미터 이름을 지켜야 함
}
```

옵셔널 위치 파라미터
- 전달하지 않아도 되는 위치 파라미터
```dart
void greet(String name, [int age = 0]) {
  print('이름: $name, 나이: $age');
}

void main() {
  greet('강준석');       // 나이 생략 가능
  greet('강준석', 29);    // 나이도 전달 가능
}

결과 :
	이름: 강준석, 나이: 0
	이름: 강준석, 나이: 29

```

---

### 조건문

- `if` :  조건이 true 일 때 실행
- `else if` :  다른 조건을 추가할 때
- `else` : 위 조건에 모두 해당하지 않을 때

```dart
void main(){
	int score = 85;
	if (score >= 90){
		print('A');
	}else if(score >= 80){
		print('B');
	} else {
		print('C 이하');
	}
}
```

### 반복문 (for/while)

- for 문
```dart
void main() {
	// 초기값 ; 조건 ; 증감식
  for (int i = 1; i <= 5; i++) {
    print('안녕하세요 $i');
  }
}
```


- while 문
```dart
void main() {
  int i = 1;
  
	// true 일동안 실행
  while (i <= 5) {
    print('while 반복: $i');
    i++;
  }
}
```


---

### 클래스 및 객체

클래스란?
- 비슷한 특징을 가진 속성과 행동들을 묶어놓은 설계도
- 클래스는 박스를 만드는 생산라인, 객체는 생산라인에서 만들어진 박스

```dart
// class(키워드) 클래스명 { 속성, 메서드 정의 }
class Person {
	// 속성
  String name;
  int age;

	// 생성자 : 클래스를 생성(객체로 만드는 일)할 때 초기화(속성에 값을 할당)하는 특별한 함수
  Person(this.name, this.age);
	
	/// 메서드(클래스 안에 있는 함수)
  void printIntro() {
    print("내이름은 $name! $age살 이지!");
  }
}

void main(){
	// 객체화
  Person joy = Person("조이", 29);
}
```

쉽게 이해하기
```dart
// hp와 attack은 마린이 가지고 있는 속성(변수)
// move()와 shoot()는 마린이 할 수 있는 행동(메서드)
class Marine {
  int hp = 40;
  int attack = 6;

  void move(String direction) {
    print("마린이 $direction 방향으로 이동합니다.");
  }

  void shoot() {
    print("탕탕! 마린이 공격합니다. 공격력: $attack");
  }
}

```

