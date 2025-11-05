
### 소개

- **Freezed**는 Flutter에서 불변(immutable) 데이터 모델을 자동 생성하는 패키지
    
- Flutter 공식 문서에서도 “불변 객체 데이터 모델” 사용을 권장
    
- **Riverpod**, **Bloc** 등 최신 상태관리 패턴과 호환성이 높음
    
- 보일러플레이트 코드를 줄여 개발 효율성과 유지보수성을 향상시킴
    

---

### 불변 객체와 상태 관리

- Freezed는 모든 필드를 자동으로 `final` 처리하여 **생성 후 수정 불가능한 객체**를 만든다.
    
- 상태 관리 시 객체 내부 값을 직접 변경하지 않고, **새로운 객체를 생성해 교체**한다.
    
- 이는 **메모리 참조값이 변경되어야** 상태 관리 시스템(Riverpod, Bloc 등)이 변화를 감지하기 때문이다.
    

---

### Freezed의 주요 기능

- **모델 정의**: `@freezed` 어노테이션과 팩토리 생성자(`factory`)를 이용해 간단히 정의
    
- **copyWith**: 기존 객체의 일부 속성만 변경한 새 객체를 생성 (얕은 복사)
    
    - 중첩 객체를 수정하려면 내부 객체의 `copyWith`를 따로 호출해야 함
        
- **JSON 직렬화**: `fromJson` / `toJson` 메서드 자동 생성
    
    - JSON 키 변환은 `@JsonKey(name: 'snake_case')` 또는 `field_rename` 옵션을 통해 설정
        
- **equality 연산자**: `==` 및 `hashCode` 자동 구현으로 객체 내용 비교 가능
    

---

### Freezed 설정하기

- 필요한 패키지:
    
    - `freezed`
        
    - `freezed_annotation`
        
    - `build_runner` (개발 의존성)
        
    - `json_serializable` (JSON 직렬화 시)
        
- 파일 구조 규칙:
    
    - 원본 파일: `model.dart`
        
    - 생성 파일: `model.freezed.dart`
        
    - `model.g.dart`는 `fromJson` / `toJson` 정의 시에만 생성됨
        

---

### Build Runner 사용하기

- 코드 생성 명령:
    
    `flutter pub run build_runner build`
    
- 지속 감시 모드:
    
    `flutter pub run build_runner watch --delete-conflicting-outputs`
    
- `--delete-conflicting-outputs` 옵션은 이전 생성 파일 충돌 시 해결용
    

---

### 실용적인 팁

- VS Code의 **File Nesting** 기능으로 `.freezed.dart`와 `.g.dart` 파일을 깔끔하게 숨길 수 있음
    
- Freezed 모델은 **순수 데이터 표현(DTO)** 용도로 사용하고, 로직은 ViewModel/Notifier 등 별도 계층에 분리
    
- `@JsonKey`를 활용해 서버의 스네이크 케이스(snake_case)와 클라이언트의 카멜 케이스(camelCase) 차이를 쉽게 매핑
    

---

### Action Items

-  Freezed로 모델 클래스 구현하기
    
-  `flutter pub run build_runner build` 명령어로 코드 자동 생성 실습하기
    
-  VS Code에서 File Nesting 설정 적용하기