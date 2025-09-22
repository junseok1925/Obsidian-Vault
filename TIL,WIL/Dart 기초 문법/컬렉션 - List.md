# List

>순서가 있는 자료 구조
  다른 언어의 배열(Array)와 비슷
  **중복 값 허용 됨**

## 선언 방법
```dart
// 명시적 타입 지정
List<String> student = ["강준석","철수","영희","맹구"];

// 타입 생략 -> 타입 추론 var 사용
var nums = [1,2,3,4];
```

## 주요 특징

1. **중복 값 허용**
```dart
List<String> name = ["강준석","강준석","철수","맹구"];
List<Int> nums = [1,1,2,2,3,3];
var nums = [1,1,2,2,3,3];
```

2. 크기 가변적
```dart
List<String>name["강준석","철수"];
name.add["영희"];
print(name); // 결과 : 강준석, 철수, 영희
```

3. 다른 타입의 요소 넣기 가능 - var
```dart
var fruits = {'사과', '파인애플', '오렌지', 8};
print(fruits.runtimeType);
// _HashSet<Object>
// String과 Int의 상위 클래스인 Object 출력 됨
```
## 주요 메서드

```dart
void main() {
	List<String> cars = ["포르쉐", "페라리"];
	
	// 추가
	cars.add("벤츠");
	print(cars) // 포르쉐, 페라리, 벤츠
	
	// 여러 개 추가
	cars.addAll(["벤츠","아우디"]);
	print(cars) // 포르쉐, 페라리, 벤츠, 아우디
	
	// 특정 위치에 삽입
	cars.insert(0,"벤츠");
	print(cars); // 벤츠, 포르쉐, 페라리
	
	// 삭제
	cars.remove("포르쉐");
	print(cars); // 페라리
	
	// 인덱스로 삭제
	cars.removeAt(0);
	print(cars); // 페라리
	
	// 모두 삭제
	cars.clear
	
	// 특정 요소 인덱스 반환
	print(cars.indexOf("페라리")); // 1
	print(cars.indexOf("현대"));  // -1 (없는 요소면 -1 반환)
	
	// 길이
	print(cars.length); // 2
	
	// 포함 여부 확인
	pint(cars.contains("포르쉐")); // true
	
	// empty 확인
	print(cars.isEmpty);    // false
	print(cars.isNotEmpty); // true
}
```


---


# Set

> 순서가 없는 자료 구조
> 집합과 같은 개념
> 중복 허용 안됨

## 선언 방법

```dart
// 명시적 타입 지정
Set<int> nums = {1,2,3};

// 타입 추론 var사용
var cars = {"포르쉐","페라리","멕라렌"};

// 빈 Set 생성 -> {}은 기본적으로 Map 타입이므로 주의
Set<String> emptySet = {};         // 타입 명시 안하면 Map됨
Set<String> emptySet = <String>{}; // 올바른 빈 Set
```

## 주요 특징

1. 순서 없음
```dart
var cars = {"포르쉐","페라리","멕라렌"};
print(cars)
// 출력 : {"멕라렌","페라리","포르쉐"} -> 순서가 보장되지 않음 섞에서 나옴
```

2. 중복 불가
```dart
var cars = {"포르쉐","페라리","포르쉐"};
print(cars); // 포르쉐, 페라리
```

3. 다른 타입의 요소 넣기 가능 - var
```dart
var fruits = {'사과', '파인애플', '오렌지', 8};
print(fruits.runtimeType);
// _HashSet<Object>
// String과 Int의 상위 클래스인 Object 출력 됨

```
## 주요 메서드

```dart
void main() {
  Set<String> fruits = {"사과", "바나나"};

  // 추가
  fruits.add("포도");
  print(fruits); // {사과, 바나나, 포도}

  // 여러 개 추가
  fruits.addAll({"복숭아", "딸기"});
  print(fruits); // {사과, 바나나, 포도, 복숭아, 딸기}

  // 삭제
  fruits.remove("바나나");
  print(fruits); // {사과, 포도, 복숭아, 딸기}

  // 포함 여부
  print(fruits.contains("포도")); // true

  // 길이
  print(fruits.length); // 4

  // 비우기
  fruits.clear();
  print(fruits); // {}
}
```

## 집합 연산 - Set 장점

```dart
var a = {1, 2, 3};
var b = {3, 4, 5};

// 합집합
print(a.union(b));        // {1, 2, 3, 4, 5}

// 교집합
print(a.intersection(b)); // {3}

// 차집합
print(a.difference(b));   // {1, 2}

```

---

# Map

> Key - Value로 데이터를 저장하는 자료구조
> List처럼 index가 아닌 Key로 Value를 찾는 구조

## 선언 방법
```dart
// 명시적 선언
Map<String, Int> human = {"강준석":29, "짱구":6};
Map<String, Int> human = {"강준석":29, "짱구":"6살"};
//키,값들이 선언된 값이 동일해야함

// 타입 생략 -> 타입 추론 var 사용
var human = human = {"강준석":29, "짱구":"6살"};
// 타입추론의 경우 키,값들의 타입이 통일 되지 않아두됨

```

## 주요 특징

1. Key 값 중복 불가
```dart
Map<String, Int> human = {"강준석":29, "짱구":6, "강준석":6};
// value는 중복가능, key는 중복불가
```

2. Key, Value 타입 지정 가능
```dart
Map<int, String> idToName = {1: "철수", 2: "영희"};
```

3. **제네릭 생략 시 `dynamic`**
```dart
var mix = {1: "one", "two": 2}; // Key도 Value도 dynamic
```