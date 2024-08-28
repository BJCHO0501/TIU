> 이 문서는 React-Native 0.75v을 기준으로 작성되었습니다.

## 설치 및 기본 세팅
https://reactnative.dev/docs/set-up-your-environment?os=macos&platform=android
- 공식 문서를 참고하여 세팅 진행

## 파일 생성
``` bash
npx @react-native-community/cli init {프로젝트 명}

code {프로젝트 명}
```
- 원하는 경로의 폴더에서 위 명령어 실행 시 알아서 프로젝트를 생성해줌.


## 실행 시키기
### 의존성 설치
** **실행 순서가 중요함** **
### 1. iOS 의존성 설치

``` bash
cd ios/ && pod install
```

### 2. 앱 실행
``` bash
npx react-native run-[ios / android]
```
- 꼭 위 명령어로 실행해야 오류가 발생하지 않는다.

![[Pasted image 20240828124306.png]]
- 해당 화면에서 원하는 OS 종류 선택 (iOS -> i, Android -> a)

![[Pasted image 20240828143937.png]]

## 문제 및 오류 대응
``` bash
npx react-native doctor 
```
- 위 명령어로 전체적인 세팅 오류 및 경고 사항들을 확인할 수 있음.
```bash
pod deintegrate

pod install
```
- pod 관련 문제 발생시, ios 폴더에서 위 명령어 실행
