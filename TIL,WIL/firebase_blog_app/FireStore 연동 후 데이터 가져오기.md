
# post_repository.dart

```dart
import 'package:cloud_firestore/cloud_firestore.dart';

class PostRepository {
  Future<void> getAll() async {
    // 1. 파이어스토어 인스턴스 가져와서 생성
    final firestore = FirebaseFirestore.instance;
    // 2. post 컬렉션 참조 만들기
    final collectionRef = firestore.collection('posts');
    // 3. 모든 문서 가져오기
    final result = await collectionRef.get();
	// 4. QuerySnapshot 안의 문서 목록 가져오기
    final docs = result.docs;

    for (var doc in docs) {
      print(doc.id); // 문서 고유 번호
      print(doc.data()); // 실제 데이터
    }
  }
}

```


# post.dart( model 클래스)

```dart

//Firestore에 저장되는 “하나의 게시글” 구조를 Dart 객체로 표현
class Post {
  String id;
  String title;
  String content;
  String writer;
  String imageUrl;
  DateTime createdAt;

  Post({
    required this.id,
    required this.title,
    required this.content,
    required this.writer,
    required this.imageUrl,
    required this.createdAt,
  });

  // 1. fromJson 네임드 생성자 만들기
  // Firestore에서 받은 JSON(Map<String, dynamic>) 데이터를 Dart 객체로 변환
  Post.fromJson(Map<String, dynamic> map)
    : this(
        id: map['id'],
        title: map['title'],
        content: map['content'],
        writer: map['writer'],
        imageUrl: map['imageUrl'],
        createdAt: DateTime.parse(map['createdAt']),
      );

  // 2. toJson 메서드 만들기
  // Post 객체를 Firestore에 set()이나 add()할 때 사용할 수 있는 JSON 형태로 반환
  Map<String, dynamic> toJson() {
    return {
      'id': id,
      'title': title,
      'content': content,
      'writer': writer,
      'imageUrl': imageUrl,
      'createdAt': createdAt,
    };
  }
}

```