
기본적인 레이아웃 생성

![[Pasted image 20251104195512.png]]

```dart
import 'package:flutter/material.dart';

class HomePage extends StatefulWidget {
  @override
  State<HomePage> createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  TextEditingController textEditingController = TextEditingController();

  @override
  void dispose() {
    //TextEditingController 사용시에는 반드시 dispose 호출해줘야 메모리에서 소거됨 (최적화)
    textEditingController.dispose();
    super.dispose();
  }

  void onSearch(String text) {
    print('onSearch');
  }

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: () {
        FocusScope.of(context).unfocus();
      },
      child: Scaffold(
        appBar: AppBar(
          actions: [
            GestureDetector(
              onTap: () {
                onSearch(textEditingController.text);
              },
              // 버튼의 터치영역은 44 디바이스 픽셀 이상으로 해줘야 함
              child: Container(
                width: 50,
                height: 50,
                color: Colors
                    .transparent, //  투명한 컬러주기 (컨테이너에 배경색이 없으면 자녀위젯에만 터치 이벤트가 적용 됨)
                child: Icon(Icons.search),
              ),
            ),
          ],
          title: TextField(
            maxLines: 1,
            onSubmitted: onSearch,
            controller: textEditingController,
            decoration: InputDecoration(
              hintText: '검색어를 입력하시오.',
              // TextField가 포커스를 받지 않을 때
              enabledBorder: OutlineInputBorder(
                borderRadius: BorderRadius.circular(10),
                borderSide: BorderSide(color: Colors.grey),
              ),
              // TextField가 포커스를 받고 있을 때
              focusedBorder: OutlineInputBorder(
                borderRadius: BorderRadius.circular(10),
                borderSide: BorderSide(color: Colors.black),
              ),
            ),
          ),
        ),
        // 격자로 아이템을 배치
        body: GridView.builder(
          padding: EdgeInsets.all(20),
          itemCount: 10, // 아이템 갯수
          gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
            crossAxisCount: 3, // 한 줄에 배치할 아이템 갯수
            childAspectRatio: 3 / 4, // 아이템 가로 세로 비율
            crossAxisSpacing: 10, // 가로 간격 10
            mainAxisSpacing: 10, // 세로 간격 10
          ),
          itemBuilder: (context, index) {
            return GestureDetector(
              onTap: () {
                // 하단 슬라이드 모달 시트
                showModalBottomSheet(
                  context: context,
                  builder: (context) {
                  //  해당 부분 따로 위젯파일로 뺄꺼임
                    return Container(
                      width: double.infinity, // 가로 최대한
                      height: 300,
                      child: Text('qwe'),
                    );
                  },
                );
              },
              child: Image.network('https://picsum.photos/seed/picsum/200/300'),
            );
          },
        ),
      ),
    );
  }
}


```

- `showModalBottomSheet` : 그리드 안에 사진을 클릭하면 하단 모달 시트가 나온다.
- `dispose()` : TextEditingController 사용시에는 반드시 dispose 호출해줘야 메모리에서 소거됨 (최적화)
