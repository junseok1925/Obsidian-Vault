

## 스와이프 페이지 위젯

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      home: Scaffold(
          body: PageView(
          // scrollDirection: Axis.vertical, -> 세로로 스와이프 할 수 있는 옵션
        children: [
          Container(
            color: Colors.red,
            child: const Center(
              child: Text(
                "1",
                style: TextStyle(fontSize: 50, color: Colors.white),
              ),
            ),
          ),
          Container(
            color: Colors.blue,
            child: const Center(
              child: Text(
                "2",
                style: TextStyle(fontSize: 50, color: Colors.white),
              ),
            ),
          ),
          Container(
            color: Colors.yellow,
            child: const Center(
              child: Text(
                "3",
                style: TextStyle(fontSize: 50, color: Colors.white),
              ),
            ),
          ),
        ],
      )),
    );
  }
}
```

- 스와이프로 페이지를 넘기는 위젯, 기본으로 제공되는 위젯이다.
- `Container` 요소를 더 늘려서 더 많은 페이지를 생성 할 수 있다.
- `Container`의 옵션(색상..등)을 자유롭게 커스텀 할 수 있다


==`scrollDirection: Axis.vertical` : body 영역의 PageView 옵션에 해당 코드를 넣어 세로로 스와이프 되는 페이지 옵션을 추가 할 수 있다.==


---

## Page View Controller 활용

```dart
import 'package:flutter/cupertino.dart';
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      home: Scaffold(body: SampleWidget()),
    );
  }
}

class SampleWidget extends StatefulWidget {
  @override
  State<StatefulWidget> createState() => _SampleWidgetState();
}

class _SampleWidgetState extends State<SampleWidget> {
  final _controller =
      PageController(); //- `PageController`를 `addListener`를 통해 등록하여 사용한다.

  @override
  void initState() {
    // - `initState()` : 이벤트나 상태를 초기화 할때 사용된다.
    super.initState();
    _controller.addListener(() {
      //- `PageController`를 `addListener`를 통해 등록하여 사용한다.

      //컨트롤러의 포지션의 최대 스크롤 == 현재 위치 -> 메시지 이벤트 발생
      if (_controller.position.maxScrollExtent == _controller.offset) {
        showDialog(
          context: context,
          builder: (context) =>
              const CupertinoAlertDialog(content: Text('마지막에 도달했습니다.')),
        );
      }
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: SafeArea(
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            Padding(
              padding: const EdgeInsets.all(15.0),
              child: ElevatedButton(
                // 엘리베이터 옵션
                onPressed: () {
                  _controller.jumpToPage(1); // 1(인덱스)페이지로 점프하겠다
                },
                child: Text('2페이지로 가기'),
              ),
            ),
            Expanded(
              child: PageView(
                pageSnapping: true,
                //-> 페이지 스와이프 시 절반 정도 스와이프하면 자동으로 넘어가게 함
                
                onPageChanged: (int index) { // onPageChaged 옵션 : 콜백함수 등록 가능
                  showDialog(
                    context: context,
                    builder: (context) =>
                        CupertinoAlertDialog(content: Text('$index 페이지 활성화')),
                  );
                },
                // -> 

                scrollDirection: Axis.vertical,
                controller: _controller, // 컨트롤러를 등록해야함
                children: [
                  Container(
                    color: Colors.red,
                    child: const Center(
                      child: Text(
                        "1",
                        style: TextStyle(fontSize: 50, color: Colors.white),
                      ),
                    ),
                  ),
                  Container(
                    color: Colors.blue,
                    child: const Center(
                      child: Text(
                        "2",
                        style: TextStyle(fontSize: 50, color: Colors.white),
                      ),
                    ),
                  ),
                ],
              ),
            ),
          ],
        ),
      ),
    );
  }
}


```

- `initState()` : 이벤트나 상태를 초기화 할때 사용된다.

- `PageController`를 `addListener`를 통해 등록하여 사용한다.

- `if (_controller.position.maxScrollExtent == _controller.offset)`
	-> 컨트롤러의 포지션의 최대 스크롤 == 현재 위치 -> 메시지 이벤트 발생
	
- `ElevatedButton` : 원하는 인덱스 페이지로 점프하는 옵션
	->`controller: _controller,` 컨트롤러등록 필수

- `pageSnapping: true,` -> 페이지 스와이프 시 절반 정도 스와이프하면 자동으로 넘어가게 함

- `onPageChanged` -> 이 옵션을 콜백 함수를 등록할 수 있다.
	- 해당 옵션을 사용하면 사용자가 스와이프를 이용해 페이지를 이동하거나 버튼을 통해 페이지 이동을 했을 때 콜백 함수가 호출되며 현재 보고 있는 페이지의 번호(index)를 반환해 준다. 이를 통해 각 페이지마다 특정 이벤트를 수행하거나 페이지 인디케이터를 제어할 수 있다.

