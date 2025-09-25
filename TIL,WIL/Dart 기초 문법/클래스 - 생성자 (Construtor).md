
# 생성자란?

- 클래스로부터 객체를 만들 때 자동으로 호출되는 특별한 메서드
- 주로 객체를 초기화 하는 데 사용됨
- **즉, 객체가 태어날 때 딱 한 번 불리는 메서드**


### 기본 생성자(Default Constructor)

클래스를 만들면 자동으로 제공되는 생성자
매개변수가 없고, 지정하지 않아도 사용 가능 **하지만 초기값은 있어야함**
```dart
class Person{
// String name; -> 초기값 없어서 오류
 String name = "";
 int age = 0;
}

void main() {
var p = Person(); // 기본 생성자 호출
print("${p.name}, ${p.age}") // "", 0
}
```

### 매개변수 생성자 (Parameterized Constructor)

객체를 만들 때 값을 바로 넣을 수 있는 생성자
```dart
class Persion {
	String name;
	int age;
	
	// 매개변수 생성
	Person(this.name, this.age);
}

void main() {
	var p1 = Persion('junseok', 29);
	print('${p1.name}, ${p1.age}'); // junseok, 29
}
```


### 네임드 생성자 (Named Constructor)

하나의 클래스에 여러 개의 새성자를 만들고 싶을 때 사용한다.
이름을 붙여서 구분
```dart
class Person{
	String name;
	int age;
	
	//매개변수 생성저
	Person(this.name, this.age);
	
	//네임드 생성자
	Person.young(this.name) : age = 18; //나이는 고정
	Person.old(this.name) : age = 90; // 나이는 고정
}
void main() {
	 var p1 = Person('kang', 29);
	 var p2 = Person.young('준석');
	 var p1 = Person.old('무성');
	 
	 print('${p1.name}, ${p1.age}'); // kang, 29
	 print('${p2.name}, ${p2.age}'); // 준석, 18
	 print('${p3.name}, ${p3.age}'); // 무성, 90
	 
}
```

---
### 요약

|종류|특징|예시|
|---|---|---|
|기본 생성자|매개변수 없음, 생략해도 자동 제공|`Person()`|
|매개변수 생성자|객체 생성 시 값 전달 가능|`Person(this.name, this.age)`|
|네임드 생성자|여러 개 생성자 정의 가능|`Person.young("민수")`|