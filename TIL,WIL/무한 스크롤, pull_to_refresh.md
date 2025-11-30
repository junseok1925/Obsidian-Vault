```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(home: HomePage());
  }
}

class HomePage extends StatefulWidget {
  @override
  State<HomePage> createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  // 초기 데이터 리스트 (0~19)
  List<int> items = List.generate(20, (index) => index);

  // 중복 요청 방지를 위한 플래그
  bool isFetching = false;

  // 스크롤이 끝에 닿았을 때 추가 데이터를 불러오는 함수
  void fetchMoreData() async {
    // 이미 불러오는 중이라면 중복 호출 막기
    if (isFetching) return;

    isFetching = true;

    // 지연 시간(서버 요청 시뮬레이션)
    await Future.delayed(Duration(milliseconds: 500));

    print('데이터 추가 중...');

    // 새로운 데이터 생성: 마지막 값 + 0~19
    final newList = List.generate(20, (index) => items.last + index);

    // 기존 리스트에 추가
    items.addAll(newList);

    // UI 갱신
    setState(() {});

    // 약간의 딜레이 후 isFetching false 처리
    await Future.delayed(Duration(milliseconds: 300));
    isFetching = false;
  }

  // 아래로 당기는 pull-to-refresh 처리 함수
  void onRefresh() async {
    if (isFetching) return;

    print('onRefresh 호출 됨');

    isFetching = true;

    // 새로고침 시뮬레이션 딜레이
    await Future.delayed(Duration(milliseconds: 500));

    // 데이터를 처음 상태로 리셋
    items = List.generate(20, (index) => index);

    setState(() {});

    isFetching = false;
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      // 스크롤 이벤트를 감지하기 위해 NotificationListener 사용
      body: NotificationListener(
        onNotification: (notification) {
          // 스크롤이 움직일 때마다 들어오는 이벤트
          if (notification is ScrollUpdateNotification) {
            // 현재 스크롤 위치가 맨 아래에 도달했을 때
            if (notification.metrics.pixels >=
                notification.metrics.maxScrollExtent) {
              print('fetchMoreData 호출됨');
              fetchMoreData();
            }
          }
          return false;
        },
        child: RefreshIndicator(
          // 당겼을 때 실행되는 새로고침 로직
          onRefresh: () async {
            onRefresh();
          },
          // 리스트 뷰
          child: ListView.builder(
            itemCount: items.length,
            itemBuilder: (context, index) {
              return Container(
                color: Colors.amber,
                alignment: Alignment.center,
                padding: EdgeInsets.all(20),
                margin: EdgeInsets.all(20),
                child: Text('${items[index]}'),
              );
            },
          ),
        ),
      ),
    );
  }
}

```