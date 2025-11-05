

네이버 개발자 센터(https://developers.naver.com/main/)

로그인 후 Application - 애플리케이션 등록 - 사용 API :  검색, 환경 추가 : 안드로이드(앱 패키지 이름 - com.example)

X-Naver-Client-Id , X-Naver-Client-Secret → 기억하기

documents에서 (https://developers.naver.com/docs/serviceapi/search/book/book.md#%EC%B1%85) 책 검색 api 관련 문서를 볼 수 있다.

그러면 책 검색 API를 요청할 수 있는 URL를 확인 할 수 있음 (json 타입으로)
https://openapi.naver.com/v1/search/book.json?qurey=value
→ 여기서 vlue를 “j.k롤링” 으로 요청을 보내면 “j.k롤링“ 과 관련된 책 데이터가 json타입으로 반환 된다.

http 사용을 위해 `flutter pub add http` 으로 http 라이브러리 사용

## book_repository.dart

- 외부 API를 연결하여 책 데이터를 가져오는 역할
- View나 ViewModel이 직접 API를 호출하지 않고  
    `Repository`를 통해 데이터를 요청하는 구조로 유지보수가 용이

```dart
import 'dart:convert';
import 'dart:math';

import 'package:flutter_book_search_app/data/model/book.dart';
import 'package:http/http.dart';

class BookRepository {
// Future로 나중에 비동기 작업이 완료되면 List<Book>을 반환
  Future<List<Book>> searchBooks(String query) async {
    final client = Client();
    // http 패키지의 CLient를 이용해서 GET요청 수행
    final response = await client.get(
      Uri.parse('https://openapi.naver.com/v1/search/book.json?query=$query'),
      // 네이버 OpenAPI 인증용 ID,secret 지정
      headers: {
        'X-Naver-Client-Id': 'TRS79OnXRAcUT0Wn2ycH',
        'X-Naver-Client-Secret': '5AbMeIyyfd',
      },
    );
    // Get 요청 성공 시 -> 200
    // 응답코드가 200일때 JSON 문자열 → jsonDecode로 Map 변환
    // body 데이터를 jsonDecode 함수 사용해서 map으로 바꾼 후 List<Book>으로 반환
    if (response.statusCode == 200) {
      Map<String, dynamic> map = jsonDecode(response.body);
      final items = List.from(map['items']);

      // Mappediterable 이라는 클래스에 함수를 넘겨줄테니
      // 나중에 요청하면, 그때 List(items)들을 하나씩 반복문을 돌려서
      // 내가 넘겨준 함수를 실행키셔서 새로운 리스트로 돌려줘라
      final iterable = items.map((e) {
        return Book.fromJson(e);
      });

      final list = iterable.toList();
      return list;
    }

    // 아닐때 빈 리스트 반환
    return [];
    // response.body;
    // response.statusCode;
  }
}

```


- 이 코드는 **네이버 도서 검색(Open API)**를 통해서 책 데이터를 가져오는 Repository 계층의 비동기 통신 로직이다.
- 각 항목(`e`)을 `Book` 모델로 매핑
- `.map()`은 **지연 실행(lazy)** 구조 → `.toList()`로 즉시 변환
- 결과: `List<Book>` 반환
- 실패시 `[ ]` 빈배열 반환


## Test 코드 book_repository_test.dart

```dart
import 'package:flutter_book_search_app/data/model/repository/book_repository.dart';
import 'package:flutter_test/flutter_test.dart';

void main() {
  test('BookRepository test', () async {
    //
    BookRepository bookRepository = BookRepository();
    final books = await bookRepository.searchBooks('jun');

    expect(books.isEmpty, false);
    for (var book in books) {
      print(book.toJson());
    }
  });
}

```