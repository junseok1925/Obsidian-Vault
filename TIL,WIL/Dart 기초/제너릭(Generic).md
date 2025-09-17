
제너릭이란...

- 무엇이든지 담을 수 있는 바구니
-> 다용도 바구니 : 사과, 바나나, 책 모두 보관 가능 ( 짬통 비슷..?)

- **타입을 일반화해서 재사용할 수 있게 하는 문법**
- 클래스, 메서드, 컬렉션 등을 설계할 때 **구체적인 자료형을 미리 정하지 않고** → 나중에 사용할 때 타입을 지정한다

No 제너릭 code
```dart
// class는 변수를 담는 상자이다.
// 다른 타입의 변수를 담기 위해선 여러개 class 생성이 필요하다
// -> No 제너릭의 경우 한 타입의 변수만 담을 수 있음
class IntBox {
	int content;
	Box(this.content);
}

class DoubleBox {
	double content;
	Box(this.content);
}

class StringBox {
	String content;
	Box(this.content);
}
```

Yes 제너릭 code
```dart
// T는 타입 자리 표시자
// 사용할 때 어떤 타입을 쓸지 나중에 지정합니다.
// 들어가게 되는 변수에 맞게 타입이 자동으로 지정됨.
class Box<T> {
  T content;
  Box(this.content);
}

void main(){
	var stringBox = Box<String>("Apple");
	var intBox = Box<int>(123);
}
```

**꼭 `T` 여야 하나요?**

- `T` 대신에 어떠한 이름을 써도 무관
- 하지만 가장 많이 씀

|이름|의미|사용 예|
|---|---|---|
|`T`|Type (가장 일반적인 타입 하나)|`Box<T>`|
|`E`|Element (List, Set 등에 자주 씀)|`List<E>`|
|`K`|Key (Map의 key 타입)|`Map<K, V>`|
|`V`|Value (Map의 value 타입)|`Map<K, V>`|
|`S`, `U`, etc.|기타 타입 여러 개 필요할 때|`Pair<S, T>`|

함수에도 사용 가능
```dart
T echo<T>(T value) {
  return value;
}

void main() {
  print(echo<int>(42));        // 42
  print(echo<String>("Hi"));   // Hi
}
```

### 왜 사용함

1. **타입 안정성**
    - 잘못된 타입을 넣으면 **컴파일 단계**에서 오류를 잡아줌.
    - 런타임 에러를 줄일 수 있음.
        
2. **코드 재사용성**
    - 타입마다 클래스를 따로 만들 필요 없음.
    - 한 번 제너릭으로 만들어 두면 어떤 타입이든 재사용 가능.
        
3. **가독성/유지보수성 향상**
    - 의도한 타입이 명확히 드러나서 코드 이해하기 쉬워짐.
