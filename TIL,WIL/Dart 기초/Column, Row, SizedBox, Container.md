
### Column

위에서 아래로 순서대로 배치
자식 수는 제한 없음 (화면을 넘기면 overflow 발생 가능)
부모 위젯으로부터 너비 제약이 없는 경우, `Column`은 자식 중 가장 너비가 넓은 위젯의 너비를 가잠

**주요 속성**

- `children` : 배치할 위젯들의 리스트 정의

- `mainAxisAlignment`: 수직 방향 정렬 (start, center, spaceBetween 등)
    - **start**: 위쪽 정렬
    - **end**: 아래쪽 정렬
    - **center**: 수직 가운데 정렬
    - **spaceBetween**: 위젯들 사이에 **동일한 간격**을 배치하고, **양 끝에는 간격 없음**
    - **spaceAround**: 위젯들 사이에 **동일한 간격**을 배치하고, **첫 번째 위젯 앞과 마지막 위젯 뒤에는 그 간격의 절반만큼 배치**
    - **spaceEvenly**: **모든 위젯과 위젯 사이**, 그리고 **처음과 끝에도 동일한 간격**을 배치


- `crossAxisAlignment`: 수평 정렬 (start, center, stretch 등)
    - **start**: 왼쪽 정렬
    - **end**: 오른쪽 정렬
    - **center**: 수평 가운데 정렬
    - **stretch**: 위젯들이 **교차 축 방향으로 가능한 공간을 모두 채우도록 확장**

---

### Row

자식 위젯들을 가로 방향으로 나열하는 레이아웃 위젯
- 왼쪽에서 오른쪽으로 위젯을 나열
- 자식 수 제한 없음 (화면 넘기면 overflow 발생 가능)
- 부모 위젯으로부터 높이 제약이 없는 경우, 자식 중 가장 높이가 높은 위젯의 너비를 가짐


**주요 속성**

- `children` : 배치할 위젯들의 리스트 정의
    
- `mainAxisAlignment`: 수평 방향 정렬 (start, center, spaceBetween 등)
    
    - **start**: 왼쪽 정렬
        
    - **end**: 오른쪽 정렬
        
    - **center**: 수평 가운데 정렬
        
    - **spaceBetween**: 위젯들 사이에 **동일한 간격**을 배치하고, **양 끝에는 간격 없음**
        
    - **spaceAround**: 위젯들 사이에 **동일한 간격**을 배치하고, **첫 번째 위젯 앞과 마지막 위젯 뒤에는 그 간격의 절반만큼 배치**
        
    - **spaceEvenly**: **모든 위젯과 위젯 사이**, 그리고 **처음과 끝에도 동일한 간격**을 배치


- `crossAxisAlignment`: 수직 정렬 (start, center, stretch 등)
    
    - **start**: 위쪽 정렬
    - **end**: 아래쪽 정렬
    - **center**: 수직 가운데 정렬
    - **stretch**: 위젯들이 **교차 축 방향으로 가능한 공간을 모두 채우도록 확장**

---

### SingleChildScrollView

자식 위젯 하나를 스크롤이 가능하도록 만드는 위젯

- `Column` 이나 `Row` 안의 내용이 화면보다 클 때 overflow를 방지하고 스크롤 가능하게 만든다
- 보통 `Column`과 함께 사용


**주요 속성**

- `child`: 스크롤 할 위젯 배치
- `scrollDirection`: 스크롤 방향 설정 (기본: `Axis.vertical`)

```dart
SingleChildScrollView(
	// Axis.vertical 또는 Axis.horizontal. 기본값 vertical
  // scrollDirection: Axis.vertical, 
  child: Column(
    crossAxisAlignment: CrossAxisAlignment.stretch,
    children: [
      Image.network('https://picsum.photos/200/300', fit: BoxFit.cover),
      Image.network('https://picsum.photos/200/300', fit: BoxFit.cover),
      Image.network('https://picsum.photos/200/300', fit: BoxFit.cover),
    ],
  ),

```

---

### SizedBox

간격이나 스타일링을 위한 위젯
고정된 너비와 높이를 갖는 박스를 생성하는 위젯


```dart
// 간격 만들기
SizedBox(height: 20)

// 고정 크기 안에 위젯 배치
SizedBox(
  width: 200,
  height: 100,
  child: ElevatedButton(onPressed: (){}, child: Text('Hi')),
)

// 고정 크기 안에 위젯 배치
SizedBox(
  width: double.infinity, // 무한히 확대하겠다는 의미
  height: 100,
  child: ElevatedButton(onPressed: (){}, child: Text('Hi')),
)
```


### Container

SizedBox + 레이아웃, 스타일, 정렬 등 다양한 기능을 가진 다목적 위젯

- `padding`, `margin`, `color`, `alignment`, `decoration` 등 UI 스타일 설정 가능
- _**크기가 지정되지 않으면 부모위젯의 크기를 물려받음**_

