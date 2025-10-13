# 학생 성적 관리 프로그램 (Dart)

이 프로그램은 학생의 점수를 CSV 파일로 관리하고, 학생 점수 조회, 우수학생 확인, 평균 점수 계산, 학생 등급 조회, 모든 종합 정리본 저장 기능을 제공한다.

---

## 프로젝트 구조

```
dart_flutter_assignment/
score_advanced/
├─ bin/
│ └─ score.dart # 실행 파일
├─ lib/
│ └─ score.dart # 주요 로직 및 함수
├─ data/ # CSV 파일 저장 폴더
├─ students.csv # 초기 학생 데이터 CSV
└─ README.md
```

## 실행방법

1. 터미널에서 프로젝트 `bin` 폴더로 이동:
   ```bash
   cd /bin
   ```
2. 프로그램 실행
   `dart run score.dart`

## 사용 기능

1. 우수학생 점수 확인
   최고 점수를 가진 학생을 조회

2. 전체 평균 점수 확인
   모든 학생의 평균 점수를 계산

3. 학생 점수 조회
   학생 이름 입력 → 점수 확인
   `y`선택 시 CSV 파일로 저장 가능
   저장 파일은 data 폴더에 생성

4. 프로그램 종료
   메뉴에서 4 또는 exit/종료 입력 시 종료

---

## Fix

### .csv파일에 빈줄이 있을 경우

- .csv 파일을 읽어와 안에 내용을 Map형식으로 출력하려했으나

```
학생 데이터 가져오기 실패 FormatException: 잘못된 데이터 형식:
```

-> 다음과 같은 오류가 남.

SV 파일에 빈 줄(또는 공백만 있는 줄)이 있어서 `split(',')` 결과가 `parts.length != 2`가 되어 예외가 던져진 것

```dart
final isLineEmpty = line.trim();
  if (isLineEmpty.isEmpty) {
    continue; // 빈 줄이면 다음 줄로 넘어감
  }
```

csv파일의 각 줄을 리스트로 가져는 for()문 안에 각 줄의 리스트를 값으로 담고 있는 `line.trim()` 메서드로 공백을 모두 삭제한 한 줄의 값을 가져와 `empty` 를 확인하는 로직 추가

### 파일생성 동기처리

`file.writeAsStringSync(content);` 메서드로 content를 내용으로 파일 생성 - `writeAsString` 처음에 해당 메서드를 사용했음. (비동기방식) - 파일은 생성되나 내용이 없는 빈 파일로 생성 됨. - 아직 파일 쓰기가 완료되지 않은 상태에서 프로그램이 종료되거나 다른 코드가 실행되면 - 파일은 생성되지만 내용이 비어있을 수 있다고 함

### 학생등급 조회 불가 오류

해당 기능 구현 간에 '4'입력 시 해당 기능을 바로 실행하도록 하였는데

```bash
kangjunseok@gangjunseog-ui-MacBookPro bin % dart run score.dart
============= 사용하실 기능을 선택 하시오. =============
(1: 우수 학생 점수 확인, 2: 학생 전체 평균 점수 확인, 3: 학생 점수 조회, 4: 학생 등급 조회, 5: 종료)
4
kangjunseok@gangjunseog-ui-MacBookPro bin %
```

-> 아무런 작동을 안함.
`studentData` print 시 아무것도 안뜸, student.csv 파일 경로 확인했으나 정상
-> 알고보니 종료를 위해 만든 `if (category == '4') break;` 코드에서 6이 아닌 4로 되어있었음...

---

## refactor

### math 라이브러리 사용

```dart
  // 두 값을 비교해서 더 큰 값 하나만 반환한다.
  // reduce : 모든 값을 하나씩 순서대로 꺼내서 함수 (a,b)에 넣고 누적해서 하나의 결과를 만든다
  int topScore = studentData.values.reduce((a, b) => a > b ? a : b);

  //math 라이브러리의 max 함수 사용으로 더 간편하게 가장 큰 값 구하기
  int topScore = studentData.values.reduce(math.max);
```

- `math.max` → math 라이브러리의 max 함수 사용으로 쉽게 가장 큰 값 구하기 사용

## 추가 기능 구현

- 파일저장 Y or N
- 프로그램 종료 or 계속 실행
- 실행마다 딜레이(0.5)부여하여 가독성 향상
- 학생 등급 조회
- 모든 종합 정리본 저장 (.csv)

## 추가로 구현, 고쳤으면 좋았을 점...

1.  함수 재사용

- 종합 정리본 저장 함수에서 이미 구현한 우수생, 평균, 등급 기능을 재사용해서 했으면

2. 모듈화

- 우수생, 평균, 등급 (데이터 처리), 문서 읽기/출력/저장 함수로 나눠서 구현 후 조립하여 사용했으면 좋았을 것
