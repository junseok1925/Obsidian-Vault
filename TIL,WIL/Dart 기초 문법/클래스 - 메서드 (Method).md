
# 메서드

- 클래스 안에 정의된 함수
- 클래스의 객체(인스턴스) 또는 클래스 자체와 관련된 동작을 정의한다.
- 크게 2가지로 나눌 수 있다 :
	1. 인스턴스 메서드 -> 객체마다 실행할 수 있는 동작
	2. 정적 메서드(static) -> 객체 생성 없이 클래스에서 바로 실행할 수 있는 동작


## 인스턴스 메서드 (Instance Method)

- 객체를 통해서만 호출 가능
- 객체의 인스턴스 변수 (필드)에 접근 가능
- `this` 를 통해 접근 가능
```dart
class Person{
	String name;
	static int age = 29;
	
	Person(this.name); // this로 생성자(Constructor) 생성
	
	//인스턴스 메서드
	void sayName(){

		print('이름은 $name');
	}
	
	static void sayAge(){
		print('나이는 $age');
	}
}

void main(){
	var p1 = Person('준석');
	var p2 = Person('철수');
	
	p1.sayName(); // 이름은 준석
	p2.sayName(); // 이름은 철수
	Person.sayAge(); // static은 객체 생성 안하고도 호출 가능
}
```

- 클래스 안에 선언된 인스턴스 변수는 클래스 안 모든 메서드에서 자유롭게 사용 가능
```dart
class Person {
	String name;
	int age;
	
	//생성자
	Person(this.name, this.age);
	
	//메서드1
	void myName(){
		print("Hi, my name is $name");
	}
	//메서드2
	void myAge(){
		print("Hi, my age is $age");
	}
	//메서드3
	void myNameAge(){
		//다른 메서드에서 쓴 변수 그대로 사용 가능
		myName();
		myAge();
	}
}

void main(){
	var man = Person("준석, 29");
	
	man.myName(); // "Hi, my name is 준석"
	man.myAge();  // "Hi, my age is 29"
	man.myNameAge(); // 위 메서드 둘 다 실행됨
	
}
```
-> `name`, `age`같은 인스턴스 변수는 클래스 안 모든 메서드에서 접근 가능
-> 그래서 `myName`, `myAge`, `myNameAge` 함수에서 같은 변수를 자유롭게 사용할 수 있다.
-> 즉, 클래스 안에서는 일일이 변수를 전달할 필요 없이, **이미 클래스에 소속된 인스턴스 변수를 어디서든 가져다 쓸 수 있다**.


---


## 정적 메서드 (Static Method)

- 객체에 속해 있는지 않고, 클래스 자체에 속하는 메서드
- 클래스 이름을 통해 호출한다 -> 객체를 생성하지 않고도 메서드를 호출할 수 있다.
```dart
class Person{
	static String name = 'junseok'; 
	int age = 29;
	
	static void sayName(){
		print(name);
	}
	void sayAge(){
		print(age);
	}
}

void main(){
	Person person = Person();

	Person.sayName(); // "junseok" 정적 메서드라 클래스 이름으로 접근 가능
	Person.sayAge(); // 오류 발생, 인스턴스 메서드라서
}
```

- 반대로 객체로 호출할 수 없고, `this`를 통해 호출할 수 없다. 
```dart
class Person{
	static String name = 'junseok'; 
	int age = 29;
	
	static void sayName(){
		print(name);
	}
	
	void sayThisName(){
		this.sayName; //this로 호출 불가
	}
	void sayAge(){
		print(age);
	}
}

void main(){
	Person person = Person();
	person.age = 1;
	person.sayName(); // sayName은 정적메서드라 호출불가
	person.sayAge(); //  sayAge은 인스턴스메서드라 객체를 생성해서 호출가능
}
```


---

## 차이점 정리

```dart
class Person {
  // 인스턴스 변수
  String name;
  // 정적 변수
  static int count = 0;

  // 생성자
  Person(this.name) {
    count++; // 객체가 생성될 때마다 count 증가
  }

  // 인스턴스 메서드 (객체 소속)
  void sayName() {
    print("내 이름은 $name");
  }

  // 정적 메서드 (클래스 소속)
  static void showCount() {
    print("현재 만들어진 사람 객체 수: $count");
  }
}

void main() {
  // 객체 생성
  var p1 = Person("준석");
  var p2 = Person("철수");

  // 인스턴스 메서드 사용 → 객체 필요
  p1.sayName(); // 내 이름은 준석
  p2.sayName(); // 내 이름은 철수

  // 정적 메서드 사용 → 클래스 이름으로 호출
  Person.showCount(); // 현재 만들어진 사람 객체 수: 2
}

```


|구분|인스턴스 메서드|정적 메서드|
|---|---|---|
|소속|객체|클래스|
|호출 방법|`객체명.메서드()`|`클래스명.메서드()`|
|인스턴스 변수 접근|가능 (`this.name`)|불가능|
|정적 변수 접근|가능|가능|
|목적|객체마다 다른 동작|공통 동작 (객체 수 세기 등)|
