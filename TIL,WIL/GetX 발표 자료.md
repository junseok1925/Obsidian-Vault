# 1. Riverpod

## 장점

### 1) **상태의 흐름이 명확하고 구조가 강력함**

- 의존성(Provider) 구조가 눈에 보이게 드러남
- Controller가 전역으로 흩어지지 않음
- 대형 앱에서 유지보수가 쉬움

### 2) **순수 Dart 기반 → 테스트하기 매우 좋음**

- 위젯과 상태가 강하게 결합되지 않음
- 단위 테스트 작성이 쉽다

### 3) **안전한 상태 관리**

- 컴파일 타임 에러를 적극적으로 잡음 (null safety 강함)
- 의존성 주입 자동 관리

### 4) **Rebuild 최적화 구조적으로 강함**

- 어디가 rebuild 되는지 명확
- 불필요한 rebuild를 최소화하는 API들이 존재 (`select`, `ref.watch`, `ref.read`)

### 5) **개발자 커뮤니티에서 현재 가장 추천되는 방식**

- Flutter 공식 문서에도 추천
- 대규모 서비스에서 안정적으로 사용됨

## 단점

### 1) 진입장벽이 있음

- ProviderScope, WidgetRef, StateNotifier 등 개념이 많다
- 초보자에게 난이도 ↑

### 2) 코드 양이 증가

- GetX 대비 verbose
- 로직 + Provider 설정이 길어질 수 있음

### 3) 상태가 분산되기 쉬워 아키텍처 설계가 중요함

- Provider가 너무 많아지면 프로젝트 구조가 무거워진다

---

# 2. BLoC (Business Logic Component)

## 장점

### 1) **아키텍처적으로 가장 강력한 패턴**

- 이벤트 → 로직 → 상태
- 모든 흐름이 명확하게 구분됨
- 팀 프로젝트에서 역할 분담이 매우 쉬움

### 2) **테스트 용이**

- Stream 기반으로 단위 테스트가 깔끔함

### 3) **대기업/대규모 프로젝트에서 많이 사용됨**

- Code Review 기준이 명확
- 로직이 완벽하게 UI와 분리됨

### 4) **보일러플레이트 많지만 규칙성 있음**

- 큰 프로젝트일수록 장점이 크게 발휘된다

## 단점

### 1) 코드가 길고 복잡함

- State, Event, Bloc 파일을 따로 만들어야 함
- 초반 개발 속도 느림

### 2) 작은 프로젝트에는 오버엔지니어링

- “할 필요 없는 복잡함”이 생김

### 3) Stream 기반이라 실수하면 메모리 누수 발생 가능

- BlocProvider 덮어쓰기 등 실수 많음

---

# 3. GetX

## 장점

### 1) 매우 빠르게 개발 가능

- 코드를 최소화
- `.obs` & `Obx`는 매우 직관적
- Controller 기반이라 이해도 쉽다

### 2) 상태관리 + 라우팅 + DI 통합

- 하나의 패키지로 모든 구성 해결
- 구조 고민이 필요 없고 바로 개발 가능

### 3) 초보자에게 최적

- 러닝커브 최저
- 개인 프로젝트, MVP 개발에서 적합

### 4) 성능은 우수 (리빌드 최소화)

- Rebuild 영역이 정확히 지정됨
- 고주파 실시간 업데이트 시 버그가 적음

## 단점

### 1) 자유도가 너무 높아 구조가 망가지기 쉬움

- 아무데서나 `Get.find()` → 전역 오염
- 대형 프로젝트에서 관리가 어렵다

### 2) 테스트 코드 작성 난이도가 높음

- Business Logic이 UI와 결합되기 쉬움

### 3) 커뮤니티 트렌드가 다소 내려감

- Riverpod/BLoC 대비 엔터프라이즈 사용량 ↓

---

# 4. 실전에서 많이 나오는 비교 요약

|항목|Riverpod|BLoC|GetX|
|---|---|---|---|
|개발 속도|중간|느림|가장 빠름|
|구조 안정성|매우 좋음|최고|낮음|
|유지보수성|높음|매우 높음|낮음|
|진입장벽|중간|높음|매우 낮음|
|코드량|중간|많음|매우 적음|
|대규모 프로젝트|최적|최고|비추천|
|소규모 / 개인 프로젝트|좋음|과함|최적|
|실시간 업데이트|우수|우수|매우 우수|
|테스트 용이|매우 좋음|매우 좋음|낮음|

---

# 5. 결론

### 개인 개발 / MVP / 빠르게 만들어야 하는 앱

→ **GetX**

### 중형~대형 규모 + 팀 개발 + 유지보수 중시

→ **Riverpod**

### 기업급 아키텍처 + 명확한 분리 + 장기 프로젝트

→ **BLoC**

---


CoinRush 발표 자료 – GetX 선택 근거 완성본  
**(Bloc / Riverpod 비교 + 반박 논리 포함 최신 버전)**1. 프로젝트 소개 – CoinRush란 무엇인가?  
![:두꺼운_확인_표시:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/apple-medium/2714-fe0f.png) 실시간 가상화폐 모의투자 앱  

- 시세(WebSocket)는 초당 수십 번 변함
- 지갑/포트폴리오/주문 화면이 모두 전역 상태로 연결
- 주문 시 “현재가 + 입력 수량 + 잔고 + 주문 유형”이 실시간 결합
- API, WebSocket, Auth 등 DI가 복잡

→ **즉, “실시간성 + 다중 전역 상태 + 복잡한 비즈니스 로직”**을 모두 해결해야 하는 고난도 앱  
![:오른쪽_화살표:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/apple-medium/27a1-fe0f.png) 핵심 질문:  
 **이 복잡한 상태 구조를 어떤 상태관리로 풀어야 가장 효율적일까?**  
 → 우리의 결론: **GetX**2. CoinRush의 핵심 난제 4가지  
**난제 1) 초당 수십 번 바뀌는 실시간 가격 데이터**  

- WebSocket 가격이 계속 바뀜
- 리스트 스크롤 중에도 버벅임 없어야 함
- 특정 타일만 업데이트 필요(전체 rebuild 금지)

**난제 2) 전역 상태 동기화**  

- 지갑 잔고는 주문/홈/포트폴리오에서 **동시에 즉시 반영**
- 화면 간 상태 공유 필수

**난제 3) 주문 로직의 복잡성**  

- “현재가(Stream) + 입력값(state) + 잔고”가 실시간 결합
- 잔고 부족, 최소 금액 등 다양한 검증 존재
- UI/로직 분리가 어려움

**난제 4) 의존성 분리 + 유지보수 가능한 구조 필요**  
필수 서비스:  

- AuthService
- WebSocketService
- CoinRepository
- OrderRepository

→ 뷰에서 완전히 분리해야 유지보수 가능3. GetX는 이 4가지 난제를 어떻게 해결하는가?  
 난제 1) 실시간 시세 → GetX의 초경량 reactive 구조로 해결  
GetX 방식  

Obx(() => Text("${controller.price.value}"));

- `.obs` 값이 바뀔 때 **해당 위젯만** 리빌드
- StreamBuilder 필요 없음
- 리스트 20개가 초당 여러 번 바뀌어도 성능 유지

Bloc 비교  

- Event → Bloc → State 반복
- 이벤트 폭발 문제
- 성능 튜닝 난이도 ↑

Riverpod 비교  

- StreamProvider 재구성 잦음
- Provider dependency 많아짐

![:오른쪽_화살표:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/apple-medium/27a1-fe0f.png) **실시간 고빈도 업데이트는 GetX가 가장 효율적** 난제 2) 전역 상태 동기화 → Controller 기반 구조로 즉시 해결  
GetX 방식  

wallet.updateBalance(newBalance);
portfolio.updateTotalAsset();

- Controller 전역 공유
- Obx가 자동으로 모든 화면 갱신
- MultiProvider, ProviderScope 필요 없음

Bloc 비교  

- 전역 Bloc 구성 복잡
- 여러 Bloc 간 동기화 난이도 ↑

Riverpod 비교  

- Provider 계층 설계 필요
- Provider dependency 복잡해짐

![:오른쪽_화살표:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/apple-medium/27a1-fe0f.png) CoinRush처럼 “지갑이 모든 화면에 반영되어야 하는 앱”에 최적화 난제 3) 주문 로직의 실시간 결합  
GetX 방식  

total.value = amount.value * currentPrice.value;
isValid.value = wallet.balance >= total.value;

UI:  

Obx(() => ElevatedButton(
  onPressed: c.isValid.value ? c.submitOrder : null,
));

비교  
항목BlocRiverpodGetX상태 변화 처리Event → State 반복Provider 여러 개 watchController 내부에서 직접 연산파일 수많음많음매우 적음개발 난이도높음중~높음가장 낮음  
![:오른쪽_화살표:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/apple-medium/27a1-fe0f.png) 실시간 계산 + 검증이 많은 주문 화면은 GetX가 압도적 난제 4) DI 구조(서비스 분리)  
GetX Bindings  

class OrderBinding extends Bindings {
  void dependencies() {
    Get.put(OrderController());
    Get.put(OrderRepository());
    Get.put(WebSocketService());
  }
}

- DI 자동화
- 화면 진입 시 필요한 서비스 자동 주입
- 유지보수 단순

Bloc & Riverpod 비교  

- Bloc: RepositoryProvider + MultiBlocProvider 등 설정 복잡
- Riverpod: Provider override, dependency graph 필요

![:오른쪽_화살표:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/apple-medium/27a1-fe0f.png) **GetX DI 구조가 가장 단순하고 직관적**4. GetX 핵심 철학 3가지 (왜 CoinRush와 궁합이 완벽한가?)  
**1) PERFORMANCE**  

- Streams/ChangeNotifier 미사용
- 초경량 Reactive 엔진
- 필요한 위젯만 리빌드
- 위젯 트리 의존 제거

![:오른쪽_화살표:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/apple-medium/27a1-fe0f.png) 실시간 시세 처리 최적화**2) PRODUCTIVITY**  

- 문법 간단
- Event/State 구조 필요 없음
- Obx 한 줄로 UI 바인딩
- 라우팅·DI·국제화 내장

![:오른쪽_화살표:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/apple-medium/27a1-fe0f.png) 개발 속도 압도적 1등**3) ORGANIZATION**  

- View / Presentation / Business Logic / DI / Routing 완전 분리
- BuildContext 없어도 라우팅 가능
- MultiProvider 불필요
- Bindings로 DI 자동화

![:오른쪽_화살표:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/apple-medium/27a1-fe0f.png) 구조 단순 + 유지보수 쉬움5. Riverpod · Bloc과의 ‘실전 관점’ 비교 요약  
항목RiverpodBlocGetX개발 속도중간느림**가장 빠름**구조 안정성매우 좋음최고낮음(규칙 없으면 위험)유지보수성높음매우 높음낮음(구조화 필요)진입장벽중높음매우 낮음코드량중간많음매우 적음대규모 프로젝트최적최고비추천개인/MVP좋음과함**최적**실시간 업데이트우수우수**매우 우수**테스트 용이매우 좋음매우 좋음낮음6. GetX단점  
단점 1) “GetX는 구조가 약해서 프로젝트가 커지면 망가진다.”  
![:오른쪽_화살표:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/apple-medium/27a1-fe0f.png) **우리의 답변**  

- 우리는 Controller를 Domain 단위로 분리
    - PriceController
    - TradeController
    - PortfolioController
    - ThemeController
- 전역 상태가 필요한 CoinRush 특성상
-  **GetX의 싱글톤 컨트롤러 구조가 가장 맞다**
- Bloc/Riverpod 구조는 지나치게 무겁다

단점 2) “GetX는 테스트가 어렵다.”  
![:오른쪽_화살표:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/apple-medium/27a1-fe0f.png) **우리의 답변**  

- CoinRush 1차 MVP에서 가장 중요한 건
-  **테스트보다 실시간 UI/성능/개발 속도**
- 테스트가 필요한 영역은 Service(API/Repo)이고
-  이는 GetX와 무관하게 pure Dart 테스트 가능

단점 3) “전역 상태 남발하면 DI 꼬인다.”  
![:오른쪽_화살표:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/apple-medium/27a1-fe0f.png) **우리의 답변**  

- 우리는 Domain 기반 Controller 구조로 재정의
- Get.put 남용 금지 → Module 단위로 분리
- CoinRush는 오히려 **전역 상태 공유가 핵심 요구사항**
-  (지갑 잔고 여러 화면 즉시 반영)

단점 4) “GetX는 트렌드에서 밀렸다.”  
![:오른쪽_화살표:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/apple-medium/27a1-fe0f.png) **우리의 답변**  

- Riverpod/BLoC은 ‘대규모·장기·팀 개발’에 최적화
- 하지만 CoinRush는
-  **실시간 처리 + 빠른 반복 개발 + 1차 배포 속도**가 우선
- 트렌드보다 중요한 것은
-  “우리 프로젝트 요구사항에 가장 적합한 기술”

7. 최종 결론: 왜 CoinRush는 GetX가 최적화인가?  
![:두꺼운_확인_표시:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/apple-medium/2714-fe0f.png) 실시간 고빈도 업데이트  
→ GetX reactive 엔진이 가장 빠름  
![:두꺼운_확인_표시:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/apple-medium/2714-fe0f.png) 전역 상태 동기화가 많은 앱 구조  
→ Controller 기반 구조가 가장 단순  
![:두꺼운_확인_표시:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/apple-medium/2714-fe0f.png) 복잡한 주문 계산 로직  
→ Bloc/Riverpod 대비 파일·코드량 1/5  
![:두꺼운_확인_표시:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/apple-medium/2714-fe0f.png) DI 자동화  
→ 유지보수 + 확장성 우수  
![:두꺼운_확인_표시:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/apple-medium/2714-fe0f.png) 개발 속도  
→ 실시간 앱에서 결정적 우위  
 → 아이디어 → 구현까지 시간이 가장 짧음8. “정리하면, CoinRush는 왜 GetX를 선택했는가?”  

8. 실시간 가격 변화에 대응하는 가장 효율적 엔진
9. 전역 상태 동기화가 자연스럽고 단순
10. 주문 로직과 복잡한 검증을 가장 쉽게 구현
11. DI/라우팅까지 프레임워크 내부에서 해결
12. MVP 단계에서 개발 속도·반응성 측면에서 최적
13. Riverpod/BLoC보다 구조적 부담이 현저히 적음

![:오른쪽_화살표:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/apple-medium/27a1-fe0f.png) **“실시간 트레이딩 앱의 특성과 MVP 목표를 고려했을 때 GetX가 가장 실전적인 선택이었다.”**




---
1장
먼저 CoinRush가 어떤 앱인지 설명드리겠습니다.

CoinRush는 **실시간 가상화폐 시세 기반의 모의 투자 앱**입니다.  
특징은 크게 네 가지입니다.

첫 번째, 시세 데이터가 **초당 단위로 변화합니다.**  
특정 코인의 가격이 1초 안에 여러 번 변할 수 있으며  
이 변화가 곧바로 UI에 반영되어야 합니다.

두 번째, 사용자의 자산 상태가 여러 화면에서 **동시에 반영**되어야 합니다.  
예를 들어 주문 화면에서 코인을 매수하면  
포트폴리오 화면·홈 화면에서도 즉시 잔고가 변경되어야 합니다.

세 번째, 주문 시에는 단순히 가격만 계산하는 게 아니라  
**현재가(Stream)**, **입력한 수량(State)**, **사용자의 잔고(State)**가  
복합적으로 결합되어 실시간으로 계산이 이루어져야 합니다.

즉 CoinRush는 일반적인 정보 앱이 아니라  
'상태(State)가 굉장히 빠르게 변하고, 복잡하게 얽혀 있는 앱'이라는 점이 핵심입니다.”


2장
“이제 CoinRush가 가진 **4가지 기술적 난제**를 정리해보겠습니다.

첫 번째 난제는 **초당 수십 번 바뀌는 실시간 시세 처리**입니다.  
WebSocket으로 가격이 계속 들어오는데,  
UI가 이를 버벅임 없이 보여줘야 한다는 점이 매우 까다롭습니다.  
특히 리스트뷰 스크롤 중에도 프레임 드롭 없이 동작해야 합니다.

두 번째 난제는 **지갑/포트폴리오의 전역 상태 동기화**입니다.  
내 잔고는 주문 화면·홈 화면·포트폴리오 화면 모두에서 공유되고  
어느 화면에서든 상태가 변하면 즉시 전체에 반영되어야 합니다.

세 번째 난제는 **매수/매도 로직의 복잡성**입니다.  
현재가 Stream과 입력된 수량, 그리고 내 잔고가 실시간으로 결합되어  
주문 가능 여부를 판단해야 하고,  
잔고 부족, 최소 주문 금액, 시장가/지정가 등  
조건 분기까지 모두 반영되어야 합니다.

마지막 네 번째는 **의존성 분리와 구조적 유지보수성**입니다.  
AuthService, WebSocketService, CoinRepository, OrderRepository 같은  
핵심 서비스들은 모두 View와 분리되어야 하고  
의존성 주입을 통한 깔끔한 구조가 필요합니다.

즉 CoinRush는 단순 상태관리가 아니라  
‘실시간 반응성’과 ‘여러 상태의 결합’이 매우 중요한 앱입니다.”

3장

방금 말씀드렸던 난제 중 첫 번째,  
**초당 수십 번 바뀌는 실시간 시세 처리’**를 어떻게 해결했는지 설명드리겠습니다.

GetX의 핵심은 바로 **반응형 상태관리(.obs)** 입니다.

우리가 만드는 PriceController를 보시면  
코인 리스트를 `coins = <Coin>[].obs`로 선언하고 있습니다.  
이렇게 선언된 리스트의 특정 index만 수정하면  
GetX는 **그 값과 연결된 Obx 위젯만 최소 단위로 다시 빌드합니다.**

즉, 아래 코드처럼 리스트에서 특정 코인의 가격만 업데이트할 때

`coins[index] = newCoin;`

전체 화면을 빌드하지 않고  
**해당 코인 카드를 렌더링하는 PriceTile만 다시 그립니다.**

UI는 다음처럼 구성됩니다.

`Obx(() => ListView.builder(   itemCount: c.coins.length,   itemBuilder: (_, i) => PriceTile(c.coins[i]), ))`

그래서 초당 수십 번 WebSocket으로 가격이 들어와도  
리스트 자체는 부드럽게 스크롤되고 버벅임이 없습니다.

즉 GetX는 CoinRush의 핵심 요구 중 하나인  
미친 듯이 빠른 실시간 반응성’을 가장 가볍고 빠르게 구현할 수 있는 기술입니다.


4장
이번 장에서는 CoinRush에 적용 가능한 세 가지 대표적인 상태관리 기술,  
Bloc, Riverpod, 그리고 GetX를 **실시간 시세 처리 성능 중심으로 비교**해보겠습니다.

먼저 Bloc입니다.  
Bloc는 Event → State 구조가 명확하고 유지보수성이 높다는 장점이 있지만,  
CoinRush처럼 가격이 **초당 수십 번 변화하는 환경에서는 문제가 발생합니다.**  
가격이 한 번 변할 때마다 Event가 발생하고 Stream이 계속 흘러가며  
StreamBuilder가 여러 번 재빌드되기 때문에  
이벤트 폭증과 스트림 과부하가 일어날 수 있습니다.  
즉, 실시간 고주파 UI에서는 Bloc의 구조적 장점이 오히려 부담이 됩니다.

다음은 Riverpod입니다.  
Riverpod은 구조적으로 안전하고 테스트성이 좋지만,  
반대로 상태 변화가 많을 경우 Provider들이 **연쇄적으로 재구성(rebuild)** 되는 문제가 있습니다.  
특히 StreamProvider를 사용하면 하위 Provider까지 영향이 퍼지기 쉽고,  
최적화하기 위해 Select나 Scoped Provider 등을 활용해야 하는데  
이 과정이 까다롭습니다.  
따라서 초당 수십 번 변화하는 실시간 리스트를 Riverpod로 최적화하는 것은  
노력 대비 효율이 낮습니다.

반면 GetX는 매우 단순합니다.  
**`.obs` 값만 바뀌면 해당 위젯만 즉시 리빌드됩니다.**  
StreamBuilder를 사용할 필요도 없고,  
리스트 전체가 아니라 변경된 ‘그 한 행’만 다시 그려서  
스크롤 중에도 부드러운 성능을 유지합니다.



```dart
class TradeController extends GetxController {
  // 사용자가 입력하는 주문 수량 (예: 0.5 BTC)
  var amount = 0.0.obs;
  // 실시간으로 변동하는 현재가 (WebSocket으로 들어오는 값)
  var currentPrice = 0.0.obs;
  // amount × currentPrice 로 계산된 총 주문 금액
  var total = 0.0.obs;
  // “주문 가능한지 여부”를 나타내는 Boolean
  // 잔고가 total보다 많으면 true, 부족하면 false
  var isValid = false.obs;
  // 지갑(잔고) 컨트롤러 가져오기
  // 주문 금액과 비교하기 위해 필요
  final wallet = Get.find<WalletController>();
  // 핵심 계산 함수: 사용자가 amount를 입력하거나 가격이 변할 때마다 호출
  void calculate() {
    // 총 주문 금액 = 수량 × 현재가
    total.value = amount.value * currentPrice.value;
    // 주문 가능 여부: 잔고 >= 총 주문 금액?
    isValid.value = wallet.balance.value >= total.value;
    // 이 계산이 끝나는 즉시 Obx를 통해 UI도 자동 업데이트된다
  }
}

```