
 # TIL(Today I learend)
 
## 1. Today I Learned


### 1. 빙고 알고리즘

- (3 * 3) 빙고 판을 표현한 배열을 입력받아, 빙고인 경우 `true`, 아니면 `false`를 반환하는 함수를 작성하세요. (단, 칸이 비어있는 경우는 0, 칸이 채워져 있는 경우는 1로 표현합니다.)

예:

```js
bingo([
  [0, 1, 0],
  [0, 1, 1],
  [0, 0, 1]
]) // -> false

bingo([
  [1, 1, 0],
  [0, 1, 1],
  [0, 0, 1]
]) // -> true

bingo([
  [0, 1, 0],
  [0, 1, 1],
  [0, 1, 1]
]) // -> true
```

답:

```js
function bingo(arr) {
  // 가로줄 확인 (루프)
  for (let i = 0; i < 3; i++) {
    // '이제까지 본 것이 전부 x표시가 되어있다'
    let checked = true
    
    //안쪽 배열 하나 검사
    for (let j = 0; j < 3; j++) {
      if (arr[i][j] === 0) {
        checked = false
      }
    }
      //안쪽 배열 하나 검사 끝

    if (checked) {
      return true
    }
    //한 배열이 끝날 떄 마다 체크해서 만약 checked = true 이면 나머지 배열을 볼 필요 없이 return true 를 해준다
  }

  // 세로줄 확인 (루프)
  for (let i = 0; i < 3; i++) {
    // '이제까지 본 것이 전부 x표시가 되어있다'
    let checked = true
    for (let j = 0; j < 3; j++) {
      if (arr[j][i] === 0) {
        checked = false
      }
    }
    if (checked) {
      return true
    }
  }

  {
    // 대각선 확인 (루프)
    let checked = true
    for (let j = 0; j < 3; j++) {
      if (arr[j][j] === 0) {
        checked = false
      }
    }
    if (checked) {
      return true
    }
  }

  {
    // 반대쪽 대각선 확인 (루프)
    let checked = true
    for (let j = 0; j < 3; j++) {
      if (arr[j][2-j] === 0) {
        checked = false
      }
    }
    if (checked) {
      return true
    }
  }

  return false
}

```

이중 배열을 검사해서, check 초기값을 true 로 두고 3개가 연속되는 숫자, 즉 빙고가 아닐 경우 'false'를 반환하고 빙고일 경우 true 를 반환하는 함수.


### - 궁금한 점

대각선을 확인하는 경우 왜 다시 'let checked = true' 를 할당하는지는 모르겠다. 아마도, 다른 블록이라서 전에 블록 변수는 사라지기 때문에, 새로운 블록이 생겼을 때
다시 checked 함수를 선언해 준 것 같다.


--- 

## 2. 오목 공부하기

```js
function omok(arr) {
  // 가로줄 확인
  for (let i = 0; i < 9; i++) {
    let currentPlayer;//현제 개임을 하고 있는 변수
    let count;//바둑돌을 기억하고 있는 함수
    for (let j = 0; j < 9; j++) {
      // 새로운 플레이어를 발견했을 때
      if (currentPlayer !== arr[i][j]) {
        currentPlayer = arr[i][j]
        count = 1
      } else {
        count++
      }
 // 만약 1이나 2가 5번 연속되어 있으면
      // if ((currentPlayer === 1 && count === 5) || (currentPlayer === 2 && count === 5)) {
      if ((currentPlayer === 1 || currentPlayer === 2) && count === 5) {        
        return currentPlayer
      }
    }
  }
  for (let i = 0; i < 9; i++) {
    let currentPlayer;//현제 개임을 하고 있는 변수
    let count;//바둑돌을 기억하고 있는 함수
    for (let j = 0; j < 9; j++) {
      // 새로운 플레이어를 발견했을 때
      if (currentPlayer !== arr[j][i]) {
        currentPlayer = arr[j][i]
        count = 1
      } else {
        count++
      }
 // 만약 1이나 2가 5번 연속되어 있으면
      // if ((currentPlayer === 1 && count === 5) || (currentPlayer === 2 && count === 5)) {
      if ((currentPlayer === 1 || currentPlayer === 2) && count === 5) {        
        return currentPlayer
      }
    }
  }
  {
    let currentPlayer;//현제 개임을 하고 있는 변수
    let count;//바둑돌을 기억하고 있는 함수
    for (let j = 0; j < 9; j++) {
      // 새로운 플레이어를 발견했을 때
      if (currentPlayer !== arr[j][j]) {
        currentPlayer = arr[j][j]
        count = 1
      } else {
        count++
      }
 // 만약 1이나 2가 5번 연속되어 있으면
      // if ((currentPlayer === 1 && count === 5) || (currentPlayer === 2 && count === 5)) {
      if ((currentPlayer === 1 || currentPlayer === 2) && count === 5) {        
        return currentPlayer
      }
    }
  }
  {
    let currentPlayer;//현제 개임을 하고 있는 변수
    let count;//바둑돌을 기억하고 있는 함수
    for (let j = 0; j < 9; j++) {
      // 새로운 플레이어를 발견했을 때
      if (currentPlayer !== arr[j][8-j]) {
        currentPlayer = arr[j][8-j]
        count = 1
      } else {
        count++
      }
 // 만약 1이나 2가 5번 연속되어 있으면
      // if ((currentPlayer === 1 && count === 5) || (currentPlayer === 2 && count === 5)) {
      if ((currentPlayer === 1 || currentPlayer === 2) && count === 5) {        
        return currentPlayer
      }
    }
  }


}


omok([
  [0, 0, 0, 0, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0, 0, 0, 0],
  [0, 0, 1, 0, 0, 0, 2, 0, 0],
  [0, 0, 0, 1, 0, 0, 2, 0, 0],
  [0, 0, 0, 0, 1, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 1, 2, 0, 0],
  [0, 0, 0, 0, 0, 0, 1, 0, 0],
  [0, 0, 0, 0, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0, 0, 0, 0]
]) 

```



## 3. Today I Found / Feel

이전까지 오목과 빙고는 내가 아직 내가 하기에는 실력이 부족하다고 생각했다.
이제 배열과 루프가 조금 익숙해져서, 도전해볼만해다는 생각도 들었는데, 다행이 선생님 코드를 분석하면서 오목을 완성하니 발전했다는 생각이 들어서 기분이 좋다.
비록 시험에서는 이문제를 잘 풀지 못했지만, 지금 그 것이 중요하다는 생각이 들지는 않았다. 
이 문제를 풀면서 어떤 변수를 두개씩 두고, 하나는 내가 지금 하고 있는 play 그리고
이전 것을 기억하는 memory 를 설정해서 알고리즘 짜는 방식을 익혀서 알고리즘을 공부하는데 있어서 조금 더 발전한 기분이 든다. 계속 이런식으로 알고리즘 짜는것을 몇개 더 풀어보면서 완전히 익숙하도록 익혔으면 좋겠다.

그리고 for문 말고도 다른 루프를 이용해서 푸는 방법도 곧 업데이트 해야겠다.

