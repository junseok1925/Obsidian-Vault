![[Pasted image 20251106004247.png]]


Flutter 앱에서 **책 상세 페이지(DetailPage)** 를 구현한 것
```dart
import 'package:flutter/material.dart';
import 'package:flutter_book_search_app/data/model/book.dart';
import 'package:flutter_inappwebview/flutter_inappwebview.dart';

class DetailPage extends StatelessWidget {
  Book book;
  DetailPage({required this.book});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text(book.title, maxLines: 1)),
      body: InAppWebView(
        initialUrlRequest: URLRequest(url: WebUri(book.link)),
        // Flutter InAppWebView는 기본적으로 디바이스의 기본 user-agent (모바일용) 을 사용한다.
        // 따라서 아래 코드를 지우면 자동으로 모바일 웹으로 이동한다.

        // initialSettings: InAppWebViewSettings(
        // Windows용 Chrome 브라우저의 user agent
        //   userAgent:
        //       'Mozilla/5.0 (Linux; Android 11; Pixel 5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.0.0 Mobile Safari/537.36',
        // ),
      ),
    );
  }
}
```

- book객체의 link 데이터를 가져와 해당 책 상세페이지 웹 페이지를 띄운다
- `flutter_inappwebview` 라이브러리 설치 필요 
  `flutter pub add flutter_inappwebview`

- `user-agent`는 웹서버가 “이 요청이 PC인지 모바일인지” 판단하는 문자열이다.
  만약 직접 `userAgent:`를 지정하면
    - PC용 UA → 데스크탑 페이지
    - Android/iPhone용 UA → 모바일 페이지  
        로 구분된다.
    현재는 주석 처리되어 있어 **모바일 페이지가 자동 표시됨.**
- 따라서 모바일 환경에 맞게 모바일 페이지로 자동 표시하기 위해 주석 처리