- 화면 하단에 고정된 탭 메뉴를 만드는데 사용
- 앱에서 여러 페이지를 전환할 수 있게 함 (설정,프로필,홈...등등)
- 일반적으로 `StatefulWidget`과 함께 사용하여 선택된 탭 인덱스를 상태로 관리함
- Scaffold의 bottomNavigationBar 속성에 사용

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
      title: "BottomNavigationBar 예제",
      theme: ThemeData(primarySwatch: Colors.blue),
      home: const HomePage(),
    );
  }
}

class HomePage extends StatefulWidget {
  const HomePage({super.key});

  @override
  State<HomePage> createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  int selectedIndex = 0; // 현재 선택된 BottomNavigationBar탭 인덱스 저장

  // 각 탭별 화면
  static const List<Widget> _pages = <Widget>[
  // 각 탭별 화면 3가지
    Center(child: Text("홈 화면", style: TextStyle(fontSize: 24))),
    Center(child: Text("설정 화면", style: TextStyle(fontSize: 24))),
    Center(child: Text("마이페이지 화면", style: TextStyle(fontSize: 24))),
  ];
  // 탭 클릭 시 호출, setState() → 화면 갱신, 선택된 탭(selectedIndex) 업데이트
  void _onItemTapped(int index) {
    setState(() {
      selectedIndex = index;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text("BottomNavigationBar 예제")),
      body: _pages[selectedIndex], // 선택된 화면 표시
      bottomNavigationBar: BottomNavigationBar(
      
        currentIndex: selectedIndex, // 선택된 탭
        selectedItemColor: Colors.red, // 선택된 탭의 아이콘 색상
        unselectedItemColor: Colors.grey, // 선택되지 않은 탭의 아이콘 색상
        iconSize: 28,
        selectedLabelStyle: const TextStyle(fontWeight: FontWeight.bold),
        onTap: _onItemTapped, // 탭 클릭 시 _onItemTapped 함수 호출
        items: const [
          BottomNavigationBarItem(icon: Icon(Icons.home), label: '홈'),
          BottomNavigationBarItem(icon: Icon(Icons.settings), label: '설정'),
          BottomNavigationBarItem(icon: Icon(Icons.person), label: '마이페이지'),
        ],
      ),
    );
  }
}

```

![[Pasted image 20250919173540.png]]


#### 동작 흐름
1. 앱 실행 -> MyApp -> HomePage
2. `_pages` 리스트에서 `selectedIndex`에 맞는 화면 보여줌
3. 하단 탭 클릭시 -> `_onItemTapped`호출 -> `setState()` -> 화면 갱신


---

### IndexedStack

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
      title: "BottomNavigationBar 예제",
      theme: ThemeData(primarySwatch: Colors.blue),
      home: HomePage(),
    );
  }
}

class HomePage extends StatefulWidget {
  @override
  State<HomePage> createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  int selectedIndex = 0;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("IndexedStack 예제")),
      bottomNavigationBar: BottomNavigationBar(
        currentIndex: selectedIndex,
        selectedItemColor: Colors.blueAccent,
        // unselectedItemColor: ,
        iconSize: 24,
        selectedLabelStyle: TextStyle(fontWeight: FontWeight.bold),
        // unselectedLabelStyle: ,
        onTap: (index) {
          setState(() {
            selectedIndex = index;
          });
        },
        items: [
          BottomNavigationBarItem(icon: Icon(Icons.home), label: '홈'),
          BottomNavigationBarItem(icon: Icon(Icons.settings), label: '설정'),
          BottomNavigationBarItem(icon: Icon(Icons.person), label: '마이페이지'),
        ],
      ),
	   body: IndexedStack( //indexedStack 사용
        index: selectedIndex,
        children: [
          Container(color: Colors.amber), // index 0
          Container(color: Colors.blue), // index 1
          Container(color: Colors.cyan), // index 2
        ],
      ),
    );
  }
}

```

![[Pasted image 20250919174850.png]]


1. `IndexedStack`안에는 모든 자식 위젯이 이미 생성되어 메모리에 존재한다.
2. 화면 이동시 지정된 index 하나만 화면에 보이도록 렌더링
3. 보이지 않은 자식들은 숨겨져 있을 뿐 삭제 x, 상태유지
4. 텝 이동시 화면에 보이는 것만 바뀌고, 다른 위젯은 계속 살아 있음.

비유
- `IndexedStack` : 겹쳐진 카드 뭉치?
- 탭 이동 -> 하나의 자식 카드를 맨위로 올림 
- 나머지 카드들은 아래 숨겨져 있지만 그대로 존재
- 보이는 맨 위 카드만 교체하는 느낌
