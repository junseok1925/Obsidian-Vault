
기본적인 레이아웃 생성

![[Pasted image 20251104195512.png]]

```dart
import 'package:flutter/material.dart';
import 'package:flutter_book_search_app/ui/home/home_view_model.dart';
import 'package:flutter_book_search_app/ui/home/widgets/home_bottom_sheet.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';

	// RiverPod에서 상태변화(ref.watch)를 사용할 수 있는 statefulWidget으로 설정
class HomePage extends ConsumerStatefulWidget {
  @override
  ConsumerState<HomePage> createState() => _HomePageState();
}

class _HomePageState extends ConsumerState<HomePage> {
  TextEditingController textEditingController = TextEditingController();

  @override
  void dispose() {
    //TextEditingController 사용시에는 반드시 dispose 호출해줘야 메모리에서 소거됨 (최적화)
    textEditingController.dispose();
    super.dispose();
  }
    // 홈 뷰모델의 searchBooks 메서드 호출
  void onSearch(String text) {
    // HomeViewModel 인스턴스 접근
    ref.read(homeViewModelProvider.notifier).searchBooks(text);
  }

  @override
  Widget build(BuildContext context) {
  // Riverpod provider의 상태(HomeState)를 구독
  // 상태가 변결될 때마다 UI를 자동 빌드
    final homeState = ref.watch(homeViewModelProvider);

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
          itemCount: homeState.books.length, // 아이템 갯수
          gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
            crossAxisCount: 3, // 한 줄에 배치할 아이템 갯수
            childAspectRatio: 3 / 4, // 아이템 가로 세로 비율
            crossAxisSpacing: 10, // 가로 간격 10
            mainAxisSpacing: 10, // 세로 간격 10
          ),
          itemBuilder: (context, index) {
          // homeState.books → List<Book>형태 (즉, 책 객체들의 리스트)
            final book = homeState.books[index];
            return GestureDetector(
              onTap: () {
                // 하단 슬라이드 모달 시트
                showModalBottomSheet(
                  context: context,
                  builder: (context) {
                    return HomeBottomSheet(book: book);
                  },
                );
              },
              child: Image.network(book.image),
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


## 전체 데이터 흐름 요약
```csharp
사용자 입력 → onSearch()
             ↓
     HomeViewModel.searchBooks()
             ↓
       BookRepository.searchBooks()
             ↓
     네이버 책 API 호출 → 결과 → Book 리스트 반환
             ↓
     state = HomeState(books)
             ↓
     ref.watch(homeViewModelProvider) → GridView 리빌드

```

**Riverpod 기반으로 상태를 관리하며, 검색 → API 요청 → 상태 갱신 → UI 자동 업데이트** 흐름을 가진 완전한 검색 메인 화면