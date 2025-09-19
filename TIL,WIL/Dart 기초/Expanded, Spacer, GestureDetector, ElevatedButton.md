레이아웃에서 공간을 유연하게 차지할 때 많이 쓴다.
둘다 Row / Column 안에서만 동작한다.

### Expanded

Row, Column 내에서 남은 공간을 차지하도록 자식 위젯을 확장시켜주는 위젯
- 남은 공간을 비율대로 나눠서 채움
- 자식 위젯이 가능한 최대 크기까지 확장되도록 만듬
- 여러 개의 `Expanded`가 있을 경우, `flex` 값에 따라 공간을 나눔
- **반드시 Column, Row 내에서만 사용 가능**
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(
    MaterialApp(
      home: Scaffold(
        body: Row(
          children: [
            Expanded(flex: 2, child: Container(color: Colors.red)),
            Expanded(flex: 1, child: Container(color: Colors.yellow)),
          ],
        ),
      ),
    ),
  );
}


```

![[Pasted image 20250919134450.png]]

---

### Spacer

`Expaded`와 동일 하지만 자녀위젯은 가지지 못함
- 빈 공간만 차지함 (중간 간격용)
- 내부 child 없음 (자식 불가)
- 가운데 있는 `Spacer()`가 공간을 다 먹으면서 아이콘들을 양쪽 끝으로 밀어냄
- 결과 → 별은 왼쪽, 하트는 오른쪽 끝에 배치됨.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(
    MaterialApp(
      home: Scaffold(
        body: Center(
          child: Row(
            children: [
              Icon(Icons.star, size: 50, color: Colors.orange),
              Spacer(), // 남는 공간 차지 (자동으로 밀어냄)
              Icon(Icons.favorite, size: 50, color: Colors.red),
            ],
          ),
        ),
      ),
    ),
  );
}
```

![[Pasted image 20250919135222.png]]


---

### 동일한 간격의 무지개 출력해보기




```dart
import 'package:flutter/material.dart';

void main() {
  runApp(
    MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text("무지개")),
        body: Center(
          child: Column(
            children: [
              Expanded(flex: 1, child: Container(color: Colors.red)),
              Expanded(flex: 1, child: Container(color: Colors.orange)),
              Expanded(flex: 1, child: Container(color: Colors.yellow)),
              Expanded(flex: 1, child: Container(color: Colors.green)),
              Expanded(flex: 1, child: Container(color: Colors.blue)),
              Expanded(flex: 1, child: Container(color: Colors.purple)),
              Expanded(flex: 1, child: Container(color: Colors.indigo)),
            ],
          ),
        ),
      ),
    ),
  );
}

```

- Expanded를 사용해서 flex를 모두 1 만큼 부여하여 정해진 화면에서 동일한 간격을 가진 무지개를 구현했다.
![[Pasted image 20250919151210.png]]


정답코드
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const RainbowApp());
}

class RainbowApp extends StatelessWidget {
  const RainbowApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          backgroundColor: Theme.of(context).colorScheme.inversePrimary,
          title: const Text("무지개"),
        ),
        body: Column(
          children: [
            Expanded(child: Container(color: Colors.red)),
            Expanded(child: Container(color: Colors.orange)),
            Expanded(child: Container(color: Colors.yellow)),
            Expanded(child: Container(color: Colors.green)),
            Expanded(child: Container(color: Colors.blue)),
            Expanded(child: Container(color: Colors.indigo)),
            Expanded(child: Container(color: Colors.purple)),
          ],
        ),
      ),
    );
  }
}

```

- `StatelessWidget(RainBow)`에서 실행
- `backgroundColor: Theme.of(context).colorScheme.inversePrimary,` : 테마 사용
- `Center` 가 아닌`Column`로 감싸서 전체 화면 세로로 꽉 채워 표시
- `BuildContext` 사용으로  위젯 트리에서 내 위치를 알려주며, 테마·화면 크기·페이지 이동 같은 부모 정보를 가져올 수 있다.

----

### Stack

여러 위젯을 겹쳐서 배치할 수 있는 위젯

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const RainbowApp());
}

class RainbowApp extends StatelessWidget {
  const RainbowApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          backgroundColor: Theme.of(context).colorScheme.inversePrimary,
          title: const Text("Stack 예제"),
        ),
        body: Stack(
          alignment: Alignment.center, // 부모 박스 정가운데
          children: [
            Container(width: 200, height: 200, color: Colors.green),
            Container(width: 150, height: 150, color: Colors.blue),
            Container(
              width: 100,
              height: 100,
              color: Colors.yellow,
              child: Center(
                child: Text(
                  '노랑',
                  style: TextStyle(color: Colors.black, fontSize: 20),
                ),
              ),
            ),
            Positioned(
              right: 10,
              bottom: 10,
              child: Container(width: 50, height: 50, color: Colors.blueGrey),
            ),
          ],
        ),
      ),
    );
  }
}


```

![[Pasted image 20250919155649.png]]




---

### ElevatedBtton

- 기본 버튼 위젯

```dart



```

#### 익명함수 (Anonymous Function) 적용 부분
```dart


// 익명함수 적용 전...
void onButtonClick(){
	print("버튼 터치됨");
}

ElevatedButton(
  onPressed: onButtonClick,
  child: Text('data'),
)

// 익명함수 적용 후...
onPressed: () {
	print("버튼 클릭됨");
},
```

- 따로 함수 이름을 정의하는 방식이 아닌 즉석에서 함수 이름없이 정의한 것
- 버튼 클릭 시 실행 할 코드만 즉시 정의하여 전달 함
- 간단한, 한 번만 사용할 함수에 적용 시 유용함

---

### GestureDetector

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const GestureExampleApp());
}

class GestureExampleApp extends StatelessWidget {
  const GestureExampleApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: const Text("GestureDetector 예제")),
        body: Center(
          child: GestureDetector(
            onTap: () {
              print("이미지 터치됨");
            },
            onDoubleTap: () {
              print("이미지 두번 터치됨");
            },
            onLongPress: () {
              print("이미지 길게 터치됨");
            },
            
            // 아래 3가지 속성은 모바일로 테스트 가능한 터치 관련 속성이므로 pc에서는 작동ㄴ
			onPanUpdate: (details) {
              print("Pan 이동: ${details.delta}"); // 드래그 중 이동량
            },
            onVerticalDragStart: (details) {
              print("세로 드래그 시작: ${details.localPosition}");
            },
            onHorizontalDragUpdate: (details) {
              print("가로 드래그 이동: ${details.delta.dx}");
            },
            child: Image.network('https://picsum.photos/200/300'),
          ),
        ),
      ),
    );
  }
}


```

