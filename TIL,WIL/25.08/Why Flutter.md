# 앱 개발 방법

앱 개발 방법은 2가지가 있음

1. 네이티브 앱으로 개발

- ios : Object-C, Swift - Xcode, Mac 환경 필수
    
- Andriod : Java, Kotlin - Andriod Studio
    
    <aside> 💡
    
    성능 최적화, 각 플랫폼별로 신규 기능의 SDK가 업데이트될 때 대응이 빠름 (ios 업데이트 이후 신규 기능 적용이 빠름)
    
    </aside>
    

1. 크로스 플랫폼 앱으로 개발

- React Native : javaScript
    
- Flutter : Dart
    
    <aside> 💡
    
    ios, Android 모두 개발 가능
    
    각 플랫폼 신규 기능 SDK 업데이트 이후 크로스 플랫폼에 대한 업데이트 전에 사용이 불가능
    
    </aside>
    

# CodePush

1. 앱 배포 과정. 
	![[image 1.png]]
- 최초 앱 심사 등록 시 1주일 정도 소요

1. 크리티컬 이슈 발생 시 처리 과정 (CodePush)
	![[image 2.png]]


- 이슈 발생 시 심사 과정으로 하루 이상은 이슈 방치됨, codepush 서비스가 등장 후 위의 사진과 같이 대응이 가능해짐
- ReactNative만 지원을 했지만 Flutter에도 shorebird라는 서비스 기능 제공 됨

<aside> 💡

Flutter는 아직 성장 중인 플랫폼으로 단점이 다수 존재하지만, 대부분은 개발 전략으로 극복이 가능, 시간이 지날수록 개선되는 추세로 장기적으로 유리함.

</aside>

