
이 파일은 RiverPod 상태관리 구조에서 
`View(UI)` ←→ `ViewModel(로직)` ←→ `Repository(API)` 로 나뉘는 계층 중 ViewModel 역할을 함

즉:
- **UI(HomePage)** 는 화면을 그린다.
- **ViewModel(HomeViewModel)** 은 검색어를 받아 API를 호출하고 상태를 변경한다.
- **Repository(BookRepository)** 는 실제로 네트워크 통신을 수행한다.

```dart
// 1. 상태 클래스 만들기
import 'package:flutter_book_search_app/data/model/book.dart';
import 'package:flutter_book_search_app/data/model/repository/book_repository.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';

//화면(HomePage)에서 필요한 데이터(여기서는 `List<Book>`)를 저장
class HomeState {
  List<Book> books;
  HomeState(this.books);
}

// 2. view 모델 만들기 - Notifier 상속
// Notifier<HomeState>를 상속 → 상태(`HomeState`)를 관리하는 객체가 됨.
class HomeViewModel extends Notifier<HomeState> {

// build()는 ViewModel이 처음 만들어질 때 초기 상태를 정의한다.  
// → 여기서는 HomeState([]) (즉, “아직 검색 전”)
  @override
  HomeState build() {
    return HomeState([]);
  }

// searchBooks()는 검색 로직:
// BookRepository로부터 데이터를 가져옴.
// state를 새 HomeState(books)로 교체 → UI 자동 업데이트.
  void searchBooks(String query) async {
    final bookRepository = BookRepository();
    final books = await bookRepository.searchBooks(query);
    state = HomeState(books);
  }
}

// 3. view 모델 관리자 만들기 -NotifierProvider 객체
// ViewModel을 앱 전체에서 접근하게 하는 공급자
// 비유: Provider는 “전원 콘센트”다.  
// 어디서든 ViewModel(전기)을 꽂아서 쓸 수 있게 만들어준다.
final homeViewModelProvider = NotifierProvider<HomeViewModel, HomeState>(() {
  return HomeViewModel();
});

```

- ViewModel은 “상태를 바꾸는 리모컨.”  사용자가 버튼(검색)을 누르면 ViewModel이 리모컨처럼 Repository를 호출하고 상태를 갱신한다.

결론적으로,  
이 파일은 **“홈 화면의 상태를 관리하고, 검색 기능을 수행하는 핵심 로직 담당자”**다.  
UI는 이 파일에 의존해서 상태를 읽고, 자동으로 갱신된다.