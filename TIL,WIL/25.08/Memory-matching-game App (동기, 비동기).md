

```dart

import 'package:flutter/material.dart';
import 'package:memory_matching_game/src/card.dart';

/// 메인 게임 보드 위젯
class CardBoards extends StatefulWidget {
  CardBoards({super.key});

  @override
  State<CardBoards> createState() => _CardBoardsState();
}

class _CardBoardsState extends State<CardBoards> {
  /// 카드 값 리스트 (짝 맞추기용)
  /// 같은 숫자는 같은 카드 쌍을 의미
  List<int> cards = [1, 5, 2, 6, 3, 4, 3, 2, 6, 1, 4, 5];

  /// 카드 뒤집힘 상태 관리 리스트
  /// false → 뒷면, true → 앞면
  List<bool> cardsFlippedState = [
    false, false, false, false, false, false,
    false, false, false, false, false, false,
  ];

  /// 첫 번째 선택된 카드의 인덱스 (없으면 -1)
  int instantFirstCard = -1;

  /// 카드 클릭 시 실행되는 함수
  void onTapCard(int cardIndex) {
    print('$cardIndex 번째 카드를 선택함.');

    if (instantFirstCard == -1) {
      // 첫 번째 카드 선택
      instantFirstCard = cardIndex;
    } else {
      // 두 번째 카드 선택
      var firstCard = cards[instantFirstCard];
      var secondCard = cards[cardIndex];

      if (firstCard == secondCard) {
        // 두 카드가 같으면 성공
        print('짝이 맞는 카드입니다.');
        instantFirstCard = -1; // 선택 초기화
      } else {
        // 다르면 1초 후에 다시 뒤집기
        resetInstantCards(instantFirstCard, cardIndex);
      }
    }

    // 선택된 카드를 앞면으로 뒤집음
    setState(() {
      cardsFlippedState[cardIndex] = true;
    });
  }

  /// 틀린 두 장 카드를 1초 후에 다시 뒤집는 함수
  void resetInstantCards(int firstIndex, int secondIndex) async {
    await Future.delayed(Duration(seconds: 1)); // 1초 대기
    setState(() {
      cardsFlippedState[firstIndex] = false;
      cardsFlippedState[secondIndex] = false;
    });
    instantFirstCard = -1; // 선택 초기화
    return;
  }

  @override
  Widget build(BuildContext context) {
    return SingleChildScrollView(
      child: Wrap(
        spacing: 4,   // 카드 가로 간격
        runSpacing: 4, // 카드 세로 간격
        children: [
          for (var i = 0; i < cards.length; i++)
            CardWidget(
              isFlipped: cardsFlippedState[i], // 카드 앞/뒷면 상태 전달
              cardNumber: cards[i], // 카드 고유 숫자 전달
              onTap: () {
                onTapCard(i); // 카드 클릭 시 동작 연결
              },
            ),
        ],
      ),
    );
  }
}
```

