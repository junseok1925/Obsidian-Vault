## . 주식 앱에서 GetX가 빛나는 이유 (장점)

### ① 실시간 시세 반영의 간결함

주식 앱은 초 단위로 변하는 호가와 차트를 보여줘야 합니다. GetX의 `obs`(Reactive) 방식은 스트림(Stream) 데이터를 UI에 바인딩하는 속도가 매우 빠르고 코드가 직관적입니다.

- 소켓(Socket) 데이터가 들어올 때 `price.value = newData;` 한 줄이면 UI가 즉각 반응합니다.
    

### ② 복잡한 내비게이션 관리

주식 앱은 하단 탭(홈, 관심종목, 주문, 잔고) 간의 이동이 잦고, 특정 종목을 클릭했을 때 깊은 단계의 상세 페이지로 들어가는 경우가 많습니다.

- `Get.toNamed('/detail', arguments: stockCode)` 처럼 컨텍스트 없이 페이지를 넘나들 수 있어 내비게이션 로직이 깔끔해집니다.
    

### ③ 전역 상태 접근의 용이성

로그인한 사용자의 '자산 정보'나 '보유 종목'은 앱 어디서든 접근해야 합니다. `Get.find<UserController>()`를 통해 어떤 위젯에서든 즉시 사용자 정보를 불러올 수 있어 편리합니다.

---

## 2. 주의해야 할 점 (주식 앱의 특수성)

주식 거래 앱은 **'돈'**이 오가는 서비스이므로 GetX 사용 시 다음 사항을 반드시 고려해야 합니다.

- **상태 추적의 어려움:** GetX는 어디서든 상태를 바꿀 수 있는 '전역적' 성격이 강합니다. 대규모 프로젝트에서 "어디서 내 잔고 데이터가 수정됐지?"를 추적할 때 BLoC처럼 엄격한 패턴보다 디버깅이 힘들 수 있습니다.
    
- **메모리 오염:** 주식 앱은 오래 켜두는 경우가 많습니다. 컨트롤러가 적절히 `delete` 되지 않고 메모리에 쌓이면 앱이 무거워질 수 있으므로, GetX의 생명주기(`onInit`, `onClose`) 관리를 철저히 해야 합니다.
    

---

## 3. 주식 앱을 위한 GetX 설계 전략

만약 GetX로 주식 앱을 만드신다면, 다음과 같은 구조를 추천합니다.

### 추천 구조: 계층화 (Layering)

| **계층**         | **역할**               | **GetX 활용**                     |
| -------------- | -------------------- | ------------------------------- |
| **Data Layer** | API 통신, WebSocket 연결 | `GetConnect` 또는 `Dio` 사용        |
| **Controller** | 비즈니스 로직 (매수/매도 계산)   | `GetxController`에서 `.obs` 변수 관리 |
| **View**       | UI 렌더링               | `Obx`를 사용해 시세 실시간 업데이트          |



# 실시간 시세 업데이트 구현 예시

### controller
```dart
import 'package:get/get.dart';
import 'dart:async';

class StockController extends GetxController {
  // .obs를 붙여 반응형 변수로 만등
  var price = 72000.obs; 

  void startUpdates() {
    // 1초마다 가격이 100원씩 오르는 시뮬
    Timer.periodic(Duration(seconds: 1), (timer) {
      price.value += 100;
    });
  }
}
```

### 1. Obx 방식 (가장 보편적)
가장 간결, 그냥 변수를 갖다 쓰기만 하면 됨
- 장점 : 코드가 가장 간결, 직관
- 특징 : obx 위젯 안에서 사용된 obs 변수가 변할 때만 해당 위젯이 업뎃 됨
```dart
class ViewByObx extends StatelessWidget {
  // 이미 생성된 컨트롤러 가져오기
  final controller = Get.find<StockController>();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Obx(() => Text(
          "현재가: ${controller.price.value}원", // 변수만 적으면 끝 간결, 직관적
          style: TextStyle(fontSize: 25),
        )),
      ),
    );
  }
}
```
