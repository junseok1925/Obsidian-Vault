
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

---

## 제네릭(Generics)

자료형을 나중에 지정할 수 있는 기능
재사용성과 타입 안정성을 높여줌

- **제네릭이 없는 경우**
```dart
void main() {
	
  List nums = [1, 2, 3, 4, 5]; // dynamic 가변 객체 어떤 타입이든 추가 가능
  nums.add("abc");

  print(nums); //[1, 2, 3, 4, 5, abc] -> 숫자만 넣고싶을 때 String도 들어가게 됨
}

```

- **제네릭이 있는 경우**
```dart
void main(){
	List<Int> nums = [1,2,3,4,5];
	nums.add("abc")
	
	print(nums); // 오류발생 (타입 안전성 확보됨)
}
```

### 제네릭 함수
```dart
T getFirst<T>(List<T> items) {
	return items[0];
}

void main(){
	print(getFirst<int>([1,2,3])); // 1
	print(getFirst<String>([a,b,c])); // a
}
```
- `T` 는 타입 매개변수 : 함수 호출 시 타입을 지정할 수 있음

### 제네릭 클래스
```dart
class Box<T> {
	T value;
	Box(This.value);
}

void main(){
	var intBox = Box<int>(123);
	var stringBox = Box<String?(abc);
	
	print(intBox.value); // 123
	print(stringBox.value); //abcs
}


```

## 정리

- 함수 : 작업을 묶어서 재사용 ( 매개변수, 리턴, 화살표 함수, 선택/명명 매개변수 가능 )
- 제네릭 : 타입을 일반화시켜 코드 재사용 + 타입 안전성 확보 ( `List<T>`,`Map<T>`, 제네릭 함수 / 클래스 )