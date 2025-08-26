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

이미 설치되어 있어 패스


## Andriod Studio 설치

