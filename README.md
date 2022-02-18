# PHP wordle를 만들어보자

## 🙃 미션

Wordle은 6번의 시도로 영어 단어가 무엇인지 맞추는 게임이다.

<img width="402" alt="image" src="https://user-images.githubusercontent.com/32596517/154688302-d3e5275f-ce91-4357-a8a2-e7414d59dba6.png">

### 사전 정보
게임의 궁극적인 목표는 특정 다섯 글자 영어 단어를 추측해 맞추는 것이다. 6번의 기회가 주어지며, 각 시행마다 유효한 다섯 글자 단어를 제출하면된다. 게임 시작시 공백의 30칸짜리 타일들이 나타난다. <br />

단어를 제출하면 정답과 제출된 단어의 각 알파벳 종류와 위치를 비교해 판별한다. <br />
판별 결과는 흰색의 타일이 세가지 색(초록/노랑/회색) 중 하나로 바뀌면서 표현된다. 모든 타일이 초록색으로 바뀌면 정답을 맞춘 것이다.
- 초록색: 정답 단어의 알파벳 종류와 위치가 모두 일치 (정답: dodge 하단 사진 참고)
- 노란색: 정답 단어에 들어가는 알파벳이지만 위치가 불일치 (정답: dodge 하단 사진 참고)
- 회색: 정답 단어에 없는 알파벳 (정답: dodge 하단 사진 참고)

<img width="398" alt="image" src="https://user-images.githubusercontent.com/32596517/154688654-27bd503a-69e7-4908-b2b0-0dc382005831.png">

### 1. UI 작업
- 단어 입력 타일이 30개 존재한다.
- 키보드 UI가 하단에 존재한다.
- 공유 팝업에는 정답 여부, 플레이 타임, 공유 버튼이 존재한다.
- 색상
  - 초록색: #6aaa64
  - 노란색: #c9b458
  - 회색: #787c7e

### 2. 기능
- 실제 키보드 입력이나 하단 키보드 버튼 클릭시 해당 알파벳이 입력되어 타일에 채워진다.
- 단어의 5글자를 다 채우면 더이상 입력할 수 없다.
- 엔터를 입력하면 단어가 제출 된다.
- 단어를 5글자 다 채우지 않고 제출하면 5글자 단어만 제출할 수 있다는 알림이 표시된다.
- 5글자 단어가 없는 단어라면 존재하지 않는 단어라는 알림이 표시된다.
- 제출시 단어의 판별 결과를 http request로 받아온 후에 해당 타일들의 배경색에 반영한다. (사전 정보 참고)
- 단어의 판별 결과는 키보드 UI의 배경색에도 반영된다. (사전 정보의 사진 참고)
- 단어는 15분 마다 갱신된다.
- 만약 현재 시도중이던 정답 단어가 갱신됐다면 단어가 갱신됐다는 팝업을 띄워주고 시도 횟수를 초기화한다.
- 정답을 맞췄거나 6번 시도 했을 경우 공유 팝업이 표시된다.
- share 버튼 클릭시 아래 형식으로 텍스트가 복사된다.
  ```
  Wordle {날짜 YYYY-MM-DD} {시도 횟수}/6
  PlayTime mm:ss

  (시도한 타일들 square 이모지로 표시)
  ```

  실제 예시 (아래 처럼 복사가 되야 함)
  ```
  Wordle 2022-02-18 5/6
  PlayTime 05:52

  ⬛⬛⬛⬛🟩
  🟨⬛⬛⬛⬛
  🟩🟩⬛⬛🟨
  🟩🟩🟩🟩🟩
  ```

단어가 valid한지 체크할 때 사용할 open api: https://dictionaryapi.dev/