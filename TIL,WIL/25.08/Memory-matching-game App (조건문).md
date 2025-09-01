
- 간단한 카드 짝 맞추기 게임을 만들었다

```dart
import 'package:flutter/material.dart';
import 'package:memory_matching_game/src/card.dart';

/// 카드판을 그려주는 위젯
class CardBoards extends StatefulWidget {
  CardBoards({super.key});

  @override
  State<CardBoards> createState() => _CardBoardsState();
}

class _CardBoardsState extends State<CardBoards> {
  /// 카드들에 붙일 번호 목록
  /// 같은 숫자끼리는 같은 카드로 취급 (짝 맞추기 게임의 규칙)
  List<int> cards = [1, 5, 2, 6, 3, 4, 3, 2, 6, 1, 4, 5];

  /// 카드가 현재 뒤집혀 있는지(true) 아닌지(false)를 저장하는 리스트
  /// 초기 상태는 모두 false (즉, 전부 뒷면 상태)
  List<bool> cardsFlippedState = [
    false, false, false, false, false, false,
    false, false, false, false, false, false,
  ];

  /// 첫 번째로 선택된 카드의 인덱스를 저장
  /// -1 이면 아직 아무 카드도 선택되지 않은 상태
  int instantFirstCard = -1;

  /// 카드를 탭했을 때 호출되는 함수
  void onTapCard(int cardIndex) {
    print('$cardIndex 번째 카드를 선택함.');

    // 1) 아직 첫 번째 카드가 선택되지 않은 경우
    if (instantFirstCard == -1) {
      // 첫 번째 카드 인덱스를 저장
      instantFirstCard = cardIndex;

    } else {
      // 2) 이미 첫 번째 카드가 선택되어 있고, 두 번째 카드를 고른 경우
      var firstCard = cards[instantFirstCard]; // 첫 번째 카드 번호
      var secondCard = cards[cardIndex];       // 두 번째 카드 번호

      if (firstCard == secondCard) {
        // 두 카드 번호가 같으면 짝이 맞음
        print('짝이 맞는 카드입니다.');
        instantFirstCard = -1; // 선택 초기화
      } else {
        // 짝이 안 맞으면 두 카드 모두 다시 뒷면으로 뒤집음
        setState(() {
          cardsFlippedState[instantFirstCard] = false;
          cardsFlippedState[cardIndex] = false;
        });
        instantFirstCard = -1; // 선택 초기화
        return;
      }
    }

    // 카드가 뒤집혔다고 표시 (true로 설정)
    setState(() {
      cardsFlippedState[cardIndex] = true;
    });
  }

  @override
  Widget build(BuildContext context) {
    return SingleChildScrollView(
      child: Wrap(
        spacing: 4,     // 카드 간 가로 간격
        runSpacing: 4,  // 카드 간 세로 간격
        children: [
          // cards 리스트 길이만큼 반복해서 카드 생성
          for (var i = 0; i < cards.length; i++)
            CardWidget(
              isFlipped: cardsFlippedState[i], // 해당 카드가 뒤집혔는지 여부
              cardNumber: cards[i],            // 카드에 표시될 번호
              onTap: () {
                onTapCard(i); // 카드 클릭 시 onTapCard 실행
              },
            ),
        ],
      ),
    );
  }
}

```

- `cards` → 짝 맞추기 대상 카드들의 번호 모음. 같은 숫자끼리 짝
    
- `cardsFlippedState` → 카드가 뒤집혀 있는지 상태 관리 (`true`면 앞면, `false`면 뒷면)
    
- `instantFirstCard`→ 첫 번째로 선택한 카드 인덱스 저장
    
- `onTapCard(cardIndex)` → 카드 클릭 시 동작 처리
    
    - 첫 번째 카드면 저장.
    - 두 번째 카드면 비교 → 같으면 그대로, 다르면 다시 뒷면으로 변경
- 현재 해당 프로젝트 폴더명 수정 후 github에 올리니까… ios simulator가 작동을 안함… **iOS 시뮬레이터 빌드 과정에서 코드서명(codesign)** 라고하는데…
    
- `flutter doctor -v`→ 이상없음…
    
- `rm -rf ios` `flutter create` → ios 프로젝트 정리 후 다시 생성… 이상없음…
    
- flutter 캐쉬 정리 후 빌드 → 해결 안됨
    
    ```bash
    flutter clean
    flutter pub get
    flutter build ios --simulator
    flutter run
    ```
    
- 시간이 없어서 내일 다시 해결 예정…