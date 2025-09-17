
- 여러 개의 데이터를 묶어서 다루는 구조
	- List (리스트)
	- Set (셋)
	- Map (맵)
----
### List

```dart
List<String> fruits = ['사과','바나나','포도']; 
```

- 순서가 있음
- 중복 허용
- 인덱스로 접근 가능 : `fruits[0]` -> '사과' (그냥 자바스크립트 리스트 타입이랑 같은 듯)

주요 메서드
```dart
List<String> fruits = ['사과','바나나','포도']; 

fruits.add('오렌지'); // fruits 리스트에 '오렌지' 값 추가

fruits.remove('사과'); // fruits 리스트에 '사과' 값 삭제
fruits.removeAt(0); // fruits 리스트에 0번 인덱스 값 삭제 - '사과'

fruits.contains('포도'); // false - '포도'가 있는지 true or false

fruits.contains('바나나'); // true
```

----

### Map

- 중복없는 리스트(중복 불가)
- 순서 없음

```dart
Set<int> numbers = {1, 2, 3};
```