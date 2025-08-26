## Brew install

- 대부분 Mac에는 설치가 되어 있을 것임 homebrew사이트에 접속하여 손쉽게 설치 가능

## Flutter SDK 설치

1. [flutter sdk archive]() 페이지 접속 - 오른쪽 상단의 "Get started" 클릭
2. 개발환경에 맞는 운영체제 선택 후 ios,desktop 둘다 같은 파일이므로 ios 선택 후 파일 설치
3. `/Users/kangjunseok/development/sdk/flutter` 해당 경로에 다운 파일 압축 해제
4. 환경 변수 세팅
	1. 환경 변수 파일로 터미널 경로 접속 `vi .zshrc`
	2. " i " 로 편집 모드로 변경 후 `export PATH="$PATH:/Users/kangjunseok/development/sdk/flutter"` 입력으로 경로 설정
	3. esc -> : > wq 입력으로 입력 완료 후 저장
5. 환경변수 파일에서 나와 터미널에 `source ~/.zshrc`  -> 환경변수 리로드 
6. `flutter --version`으로 정상 작동 확인

## VScode 설치

이미 설치되어 있어 패스, VScode 설치는 손쉽게 사이트에서 설치 가능


## Andriod Studio 설치

- Andriod Studio 사이트에서 파일 설치 후 실행, 라이센스 동의 후 설치 완료.

1. flutter doctor 코드 실행으로 무결점 확인

	==Andriod doctor licenses  오류 발생 ==
```bash
kangjunseok@gangjunseog-ui-MacBookPro flutter % flutter doctor

Doctor summary (to see all details, run flutter doctor -v):

[✓] Flutter (Channel stable, 3.32.7, on macOS 15.6.1 24G90 darwin-arm64, locale ko-KR)

[!] Android toolchain - develop for Android devices (Android SDK version 36.0.0)

    **!** **Some Android licenses not accepted. To resolve this, run: flutter doctor --android-licenses**

[✓] Xcode - develop for iOS and macOS (Xcode 16.4)

[✓] Chrome - develop for the web

[✓] Android Studio (version 2025.1)

[✓] VS Code (version 1.103.2)

[✓] Connected device (2 available)

[✓] Network resources

  

! Doctor found issues in 1 category.
```

==`flutter doctor --android-licenses` 코드 터미널에 입력 후 Accept : y 모두 입력==

확인 후 다시 `flutter doctor` 입력으로 무결점 확인 후 완료.



