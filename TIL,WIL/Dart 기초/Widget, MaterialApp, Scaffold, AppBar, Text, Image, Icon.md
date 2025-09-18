
플러터에서 UI를 구성하는 모든 것의 기본 단위
텍스트, 버튼, 이미지, 스크롤 영역, 레이아웃 구성 요소 등 모든 시각적 요소는 위젯으로 표현한다.

- 위젯은 재사용 가능하고 조합 가능한 구조를 가짐
- Flutter는 위젯 트리(widget tree)를 기반으로 화면을 렌더링
- UI의 상태와 구조를 선언적으로 표현함

---

위젯은 2가지 유형으로 구분

###  StatelessWidget
- 한 번 생성되면 UI가 바뀌지 않는 위젯
- ex) 텍스트, 아이콘, 로고 등...

```dart
// StatelessWidget (추상클래스)를 상속받아서 build 메서드 재정의
class MyText extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Text('변하지 않는 텍스트');
  }
}
```


### StatefulWidget
- 사용자의 동작에 따라 UI가 바뀌는 위젯
- 내부적으로 `상태(State)`를 저장하고 `setState()`를 통해 UI를 갱싲ㄴ함
- ex) 버튼 클릭 횟수 증가, 입력 필드, 토글 등...

```dart
class CounterWidget extends StatefulWidget {
  @override
  _CounterWidgetState createState() => _CounterWidgetState();
}

class _CounterWidgetState extends State<CounterWidget> {
  int count = 0;

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Text('Count: $count'),
        ElevatedButton(
          onPressed: () {
            setState(() {
              count++;
            });
          },
          child: Text('증가'),
        ),
      ],
    );
  }
}
```

---

### MaterialApp

앱 전체의 테마, 라우팅(페이지 이동), 타이틀, 폰트 등... 설정할 수 있는 핵심 컨테이너
- **Material Design** 스타일을 적용할 수 있게 해주는 틀
- _**보통 앱당 하나만 존재**_


주요 속성
- `title` : 앱의 이름
- `home` : 첫 번째 표시할 화면 위젯
- `theme` : 전체 테마 설정
- `routes` :  라우트 설정

```dart
MaterialApp(
  title: 'Flutter Demo',
  theme: ThemeData(
    colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
  ),
  home: const MyHomePage(title: 'Flutter Demo Home Page'),
);
```

---

### Scaffold

 앱의 기본적인 화면 구조를 자는 뼈대 위젯
 
- **상단바**(AppBar), **하단바**(BottomNavigationBar), **본문**(body) 등을 담는 컨테이너
- Flutter 앱의 "화면 1개"를 구성할 때 대부분 Scaffold를 사용

`MaterialApp` 은 그림일기장 전체, `Scaffold` 는 그림 일기장 각 낱장

**주요 속성**

- `appBar`: 상단바 설정
- `body`: 화면의 주요 내용
- `floatingActionButton`: 동작 버튼
- `drawer`: 사이드 메뉴

```dart
Scaffold(
  appBar: AppBar(
    title: Text(widget.title),
  ),
  body: Center(
    child: Column(
      mainAxisAlignment: MainAxisAlignment.center,
      children: <Widget>[
        const Text('You have pushed the button this many times:'),
        Text(
          '$_counter',
          style: Theme.of(context).textTheme.headlineMedium,
        ),
      ],
    ),
  ),
  floatingActionButton: FloatingActionButton(
    onPressed: _incrementCounter,
    tooltip: 'Increment',
    child: const Icon(Icons.add),
  ), // This trailing comma makes auto-formatting nicer for build methods.
);
```

----

### AppBar

화면 상단에 나타나는 헤더 영역
타이틀, 아이콘, 액션 버튼 등을 배치할 수 있으며, `Scaffold`의 `appBar` 속성내에 사용

**주요 속성**

- `title`: 제목 표시
- `actions`: 우측 액션 버튼들
- `leading`: 왼쪽 아이콘 (예: 뒤로가기, 햄버거 메뉴 등)
- `backgroundColor`: 배경색 지정

```dart
AppBar(
  title: Text('Flutter'),
  actions: [
    IconButton(
      icon: Icon(Icons.settings),
      onPressed: () {},
    ),
  ],
);
```

---
### Text

화면에 글자를 표시

```dart
// 스타일 없이 기본 사용
Text("안녕하세요")


Text(
  '제목입니다',
  // 폰트 스타일 적용
  style: TextStyle(
    fontSize: 24,
    fontWeight: FontWeight.bold,
    color: Colors.blue,
  ),
  // 정렬. TextAlign enum 사용
  // (열거형)은 고정된 수의 상수 값들을 나타내는 특별한 종류의 클래스
  align: TextAlign.center,
	overflow: TextOverflow.ellipsis,
	maxLines: 1,
)
```