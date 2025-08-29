```dart
import 'package:flutter/material.dart';

import 'package:memory_matching_game/src/card.dart';

  

class CardBoards extends StatefulWidget {

CardBoards({super.key});

  

@override

State<CardBoards> createState() => _CardBoardsState();

}

  

class _CardBoardsState extends State<CardBoards> {

List<int> cards = [1, 5, 2, 6, 3, 4, 3, 2, 6, 1, 4, 5];
// 카드에 임의의 숫자 부여 (같은 숫자끼리는 같은 카드임을 명시)
List<bool> cardsFlippedState = [
// 카드의 뒤집힘 상태를 부여 기본 값은 false로 설정
false,

false,

false,

false,

false,

false,

false,

false,

false,

false,

false,

false,

];

int instantFirstCard = -1; // 선택된 카드 인덱스 -1 = 선택되지않음.
// 선택되지 않음
  

// 카드 선택시 호출되는 함수

void onTapCard(int cardIndex) {

print('$cardIndex 번째 카드를 선택함.');

if (instantFirstCard == -1) {

instantFirstCard = cardIndex;

} else {

// 2번째 카드 선택 시 로직 추가

var firstCard = cards[instantFirstCard]; // 첫번째 선택 카드

var secondCard = cards[cardIndex]; // 두번째 선택 카드

if (firstCard == secondCard) {

// 첫번째 선택 카드와 두번째 선택 카드가 같다면

print('짝이 맞는 카드입니다.');

instantFirstCard = -1; // 선택된 카드 초기화

} else {

setState(() {

cardsFlippedState[instantFirstCard] = false;

cardsFlippedState[cardIndex] = false;

});

instantFirstCard = -1; // 선택된 카드 초기화

return;

}

}

setState(() {

cardsFlippedState[cardIndex] = true;

}); // 상태가 변경되었음을 Flutter 프레임워크에 알림 (화면 갱신)

}

  

@override

Widget build(BuildContext context) {

return SingleChildScrollView(

child: Wrap(

spacing: 4,

runSpacing: 4,

children: [

for (var i = 0; i < cards.length; i++)

CardWidget(

isFlipped: cardsFlippedState[i],

cardNumber: cards[i],

onTap: () {

onTapCard(i);

}),

],

),

);

}

}
```

