
# 객체 지향 프로그래밍(OOP)의 핵심 개념

## 1. 상속

부모 클래스(SuperClass)의 속성과 메서드를 자식 클래스(SubClass)가 그대로 사용
```dart
class Person {
  String name;
  int age;

  Person(this.name, this.age) {
    print("Person 생성: $name, $age"); //Person 생성: 준석, 20
  }
}

class Student extends Person {
  String school;

  // Student 생성자
  Student(String name, int age, this.school) : super(name, age) {
    print("Student 생성: $school"); //Student 생성: 고등학교
  }
}

void main() {
  Student s = Student("준석", 20, "고등학교");
  print("이름: ${s.name}"); //이름: 준석
  print("나이: ${s.age}"); //나이: 20
  print("학교: ${s.school}"); //학교: 고등학교

}
```