
# post_repository.dart

```dart
import 'package:cloud_firestore/cloud_firestore.dart';
import 'package:flutter_firebase_blog_app/data/model/post.dart';

class PostRepository {
  Future<List<Post>> getAll() async {
    // 1. 파이어스토어 인스턴스 가져와서 생성
    final firestore = FirebaseFirestore.instance;
    // 2. post 컬렉션 참조 만들기
    final collectionRef = firestore.collection('posts');
    // 3. 모든 문서 가져오기
    final result = await collectionRef.get();
    // 4. QuerySnapshot 안의 문서 목록 가져오기
    final docs = result.docs;
    
    // Firebase에서 불러온 각 문서를 Post 객체로 바꾼다
    // 
    return docs.map((doc) {
      final map = doc.data(); 
      doc.id;
      final newMap = {'id': doc.id, ...map}; // 문서 Id와 나머지 요소를 합쳐 newMap 생성
      return Post.fromJson(newMap); // Post 객체로 변환
    }).toList();
  }
}
```

- `return docs.map((doc) {…}).toList();`
  -  `.map`은 각 문서(doc)를 순서대로 돌면서 새로운 값으로 바꾸는 함수
  - 모든 문서를 Post 객체로 바꿈
  - 마지막을 map의 결과물을 `List<Post>` 로 만들어 준다.

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