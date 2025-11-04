![[Pasted image 20251104204815.png]]

```dart
import 'package:flutter/material.dart';
import 'package:flutter/widgets.dart';
import 'package:flutter_book_search_app/ui/detail/detail_page.dart';

class HomeBottomSheet extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      padding: EdgeInsets.fromLTRB(20, 20, 20, 50),
      width: double.infinity, // 가로 최대한
      height: 300,
      child: Row(
        children: [
          Image.network(
            'https://picsum.photos/seed/picsum/200/300',
            fit: BoxFit.cover, // 이미지를 비율 유지하면서 영역에 꽉 채우되, 필요하면 잘라내라
          ),
          SizedBox(width: 20),
          Expanded(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start, // 왼쪽 정렬
              children: [
                Text(
                  '영화 제목 부분',
                  style: TextStyle(
                    color: Colors.blueAccent,
                    fontWeight: FontWeight.bold,
                    fontSize: 20,
                  ),
                ),
                Text(
                  '감독',
                  style: TextStyle(fontWeight: FontWeight.bold, fontSize: 15),
                ),
                Text(
                  '영화 소개 글',
                  style: TextStyle(fontWeight: FontWeight.bold, fontSize: 14),
                ),
                Spacer(),
                // DetailPage로 이동
                GestureDetector(
                  onTap: () {
                    //
                    Navigator.push(
                      context,
                      MaterialPageRoute(
                        builder: (context) {
                          return DetailPage();
                        },
                      ),
                    );
                    print('자세히보기 클릭됨');
                  },
                  child: Container(
                    height: 50,
                    width: double.infinity,
                    color: Colors.transparent,
                    alignment: Alignment.center,
                    child: Text(
                      '자세히 보기',
                      style: TextStyle(
                        color: Colors.blueAccent,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                  ),
                ),
              ],
            ),
          ),
        ],
      ),
    );
  }
}

```

TRS79OnXRAcUT0Wn2ycH

5AbMeIyyfd


https://openapi.naver.com/v1/search/book.json?qurey=value

X-Naver-Client-Id: {애플리케이션 등록 시 발급받은 클라이언트 아이디 값}
X-Naver-Client-Secret: {애플리케이션 등록 시 발급받은 클라이언트 시크릿 값}