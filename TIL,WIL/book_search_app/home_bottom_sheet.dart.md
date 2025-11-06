![[Pasted image 20251104204815.png]]



책을 탭했을 때 하단에 뜨는 상세 요약 모달 UI

```dart
import 'package:flutter/material.dart';
import 'package:flutter/widgets.dart';
import 'package:flutter_book_search_app/data/model/book.dart';
import 'package:flutter_book_search_app/ui/detail/detail_page.dart';

class HomeBottomSheet extends StatelessWidget {
  final Book book;
  HomeBottomSheet({required this.book});

  @override
  Widget build(BuildContext context) {
    return Container(
      padding: EdgeInsets.fromLTRB(20, 20, 20, 50),
      width: double.infinity, // 가로 최대한
      height: 300,
      child: Row(
        children: [
          Image.network(
            book.image,
            fit: BoxFit.cover, // 이미지를 비율 유지하면서 영역에 꽉 채우되, 필요하면 잘라내라
          ),
          SizedBox(width: 20),
          //남은 공간을 꽉 채우는 구성
          Expanded(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start, // 왼쪽 정렬
              children: [
                Text(
                  book.title,
                  maxLines: 2,
                  style: TextStyle(
                    color: Colors.blueAccent,
                    fontWeight: FontWeight.bold,
                    fontSize: 20,
                  ),
                ),
                Text(
                  book.author,
                  style: TextStyle(fontWeight: FontWeight.bold, fontSize: 15),
                ),
                Text(
                  book.description,
                  maxLines: 3,
                  style: TextStyle(fontWeight: FontWeight.bold, fontSize: 14),
                ),
                Spacer(),
                // DetailPage로 이동
                GestureDetector(
                  onTap: () {
                    Navigator.push(
                      context,
                      MaterialPageRoute(
                        builder: (context) {
                          return DetailPage(book: book);
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

