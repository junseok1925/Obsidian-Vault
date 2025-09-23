
특정 작업을 묶어서 재사용할 수 있도록 만든 코드 블럭
`입력(매개변수) -> 처리 -> 출력(리턴값) 구조`

### 함수
```dart
// 명시적 
int add(int a, int b){
	retun a + b;
}

// 리턴 타입 생략 (타입 추론)
add(a, b){
	 return a + b;
}

void main() {
	print(add(3,4)); // 8	
}

// 화살표 함수
int square(int x) => x * x;

void main(){
	print(square(4)); // 16
}
```
- `int`: 리턴 타입
- `a, b` : 매개변수
- `return` : 결과 반환

### 매개변수 종류
```dart
void greet(String name){
	print("Hello, $name");
}

// 선택 매개변수(대괄호)
void greet([String name = "junseok"]){
	print("Hello $name");
}

void main(){
	greet(); //Hello junesok
	greet("준석"); //Hello 준석
}

// 명명된 매개변수(중괄호)
void greet({String name = "junseok", int age = 29}){
	print("name: $name, age: $age");
}

void main(){
	greet(); // name: junseok, age: 29
	greet(name:"준석", age:1); // name: 준석, age: 1
	}

```