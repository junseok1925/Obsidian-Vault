
- 여러 개의 데이터를 묶어서 다루는 구조
	- List (리스트)
	- Set (셋)
	- Map (맵)
----
### List

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

### Set

- 중복없는 리스트(중복 불가) -> 중복 이름 가입 불가 등등... 활용도가 높음
- 순서 없음

```dart
Set<int> numbers = {1, 2, 3};

numbers.add(2);  // 무시됨 - 이미 2가 있음

print(numbers[0]); // 순서가 없어서 출력 안됨 - 인덱스로 접근 불가
```

---

### Map

- `key`->`value` 구주
- 키는 중복 불가, 값은 중복 가능

```dart
void main() {
  Map<String, int> score = {
    '준석': 1, // key: 준석, value: 1
    '영희': 2,
  };

	  print(score['준석']); // 1
	  score['철수'] = '3'; // '철수' = 3 추가 됨
	  score['준석'] = '4'; // '준석' = 4 로 수정 됨
	  
	  int value = score['준석']
	  print(value); // 4
  
	  score.forEach((key, value) {
		print('$key: $value');
		}); //준석: 1 영희: 2

	}

}



```