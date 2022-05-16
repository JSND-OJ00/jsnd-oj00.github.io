# Stack - 브라우저 뒤로 가기과 앞으로 가기

**문제**

개발자가 되고 싶은 김코딩은 자료구조를 공부하고 있습니다. 인터넷 브라우저를 통해 스택에 대해 검색을 하면서 다양한 페이지에 접속하게 되었는데 "뒤로 가기", "앞으로 가기"를 반복하면서 여러 페이지를 참고하고 있었습니다.

그런데 새로운 페이지를 접속하게 되면 "앞으로 가기" 버튼이 비활성화돼서 다시 보고 싶던 페이지로 갈 수 없었습니다. 이러기를 반복하다가 김코딩은 스택 자료구조를 떠올리게 되었습니다.

브라우저에서 "뒤로 가기", "앞으로 가기" 기능이 어떻게 구현되는지 궁금해진 김코딩은 몇 가지 조건을 아래와 같이 작성하였지만, 막상 코드를 작성하지 못하고 있습니다.

### **조건**

1. 새로운 페이지로 접속할 경우 prev 스택에 원래 있던 페이지를 넣고 next 스택을 비웁니다.
2. 뒤로 가기 버튼을 누를 경우 원래 있던 페이지를 next 스택에 넣고 prev 스택의 top에 있는 페이지로 이동한 뒤 prev 스택의 값을 pop 합니다.
3. 앞으로 가기 버튼을 누를 경우 원래 있던 페이지를 prev 스택에 넣고 next 스택의 top에 있는 페이지로 이동한 뒤 next 스택의 값을 pop 합니다.
4. 브라우저에서 뒤로 가기, 앞으로 가기 버튼이 비활성화일 경우(클릭이 되지 않을 경우)에는 스택에 push 하지 않습니다.

인터넷 브라우저에서 행동한 순서가 들어있는 배열 actions와 시작 페이지 start가 주어질 때 마지막에 접속해 있는 페이지와 방문했던 페이지들이 담긴 스택을 반환하는 솔루션을 만들어 김코딩의 궁금증을 풀어주세요.

## **입력**

### **인자 1: actions**

- `String`과 `Number` 타입을 요소로 갖는 브라우저에서 행동한 순서를 차례대로 나열한 배열

### **인자 2: start**

- `String` 타입의 시작 페이지를 나타내는 현재 접속해 있는 대문자 알파벳

## **출력**

- `Array` 타입을 리턴해야 합니다.

## **주의사항**

- 만약 start의 인자로 string 자료형이 아닌 다른 자료형이 들어온다면 false를 리턴합니다.
- 새로운 페이지 접속은 알파벳 대문자로 표기합니다.
- 뒤로 가기 버튼을 누른 행동은 -1로 표기합니다.
- 앞으로 가기 버튼을 누른 행동은 1로 표기합니다.
- 다음 방문할 페이지는 항상 현재 페이지와 다른 페이지로 접속합니다.
- 방문한 페이지의 개수는 100개 이하입니다.
- 반환되는 출력값 배열의 첫 번째 요소 prev 스택, 세 번째 요소 next 스택은 배열입니다. 스택을 사용자 정의한다면 출력에서는 배열로 변환해야 합니다.

## **입출력 예시**

```
1
2
3
4
5
6
7
8
9
10
11
const actions = ["B", "C", -1, "D", "A", -1, 1, -1, -1];
const start = "A";
const output = browserStack(actions, start);

console.log(output); // [["A"], "B", ["A", "D"]]

const actions2 = ["B", -1, "B", "A", "C", -1, -1, "D", -1, 1, "E", -1, -1, 1];
const start2 = "A";
const output2 = browserStack(actions2, start2);

console.log(output2); // [["A", "B"], "D", ["E"]]
```

## 풀이

```jsx
function browserStack(actions, start) {
  if (typeof start !== 'string') {
    return false
  }
  let prev = []
  let next = []
  let current = start
  for (let i = 0; i < actions.length; i ++) {
    if ( typeof actions[i] === 'string') {
      prev.push(current)
      next = []
      current = actions[i]
    }
    if ( actions[i] === -1 && prev.length !== 0) {
      next.push(current)
      current = prev.pop()
    }
    if ( actions[i] === 1 && next.length !== 0) {
      prev.push(current)
      current = next.pop()
    }
  }
  return [prev,current,next]
}
```

# Queue - 박스 포장

## **문제**

마트에서 장을 보고 박스를 포장하려고 합니다. 박스를 포장하는 데는 폭이 너무 좁습니다. 그렇기에 한 줄로 서 있어야 하고, **들어온 순서대로 한 명씩 나가야 합니다.**

불행 중 다행은, 인원에 맞게 포장할 수 있는 기구들이 놓여 있어, 모두가 포장을 할 수 있다는 것입니다. 짐이 많은 사람은 짐이 적은 사람보다 포장하는 시간이 길 수밖에 없습니다.

**뒷사람이 포장을 전부 끝냈어도 앞사람이 끝내지 못하면 기다려야 합니다.** 앞사람이 포장을 끝내면, 포장을 마친 뒷사람들과 함께 한 번에 나가게 됩니다.

만약, 앞사람의 박스는 5 개고, 뒷사람 1의 박스는 4 개, 뒷사람 2의 박스는 8 개라고 가정했을 때, 뒷사람 1이 제일 먼저 박스 포장을 끝내게 되어도 앞사람 1의 포장이 마칠 때까지 기다렸다가 같이 나가게 됩니다.

이때, 통틀어 **최대 몇 명이 한꺼번에 나가는지 알 수 있도록** 함수를 구현해 주세요.

## **입력**

### **인자 1 : boxes**

- `Number` 타입을 요소로 갖는, 포장해야 하는 박스가 담긴 배열
    - 1 ≤ 사람 수 ≤ 10,000
    - 1 ≤ 박스 ≤ 10,000

## **출력**

- `Number` 타입을 리턴해야 합니다.

## **주의 사항**

- 먼저 포장을 전부 끝낸 사람이 있더라도, 앞사람이 포장을 끝내지 않았다면 나갈 수 없습니다.

## **예시**

만약 [5, 1, 4, 6](https://urclass.codestates.com/codeproblem/10a924c1-c7fa-4cec-a0a1-02ffd2cc7190)이라는 배열이 왔을 때, 5개의 박스를 포장하는 동안 1, 4 개의 박스는 포장을 끝내고 기다리게 되고, 6 개의 박스는 포장이 진행 중이기 때문에, 5, 1, 4 세 개가 같이 나가고, 6이 따로 나가게 됩니다. 그렇기에 최대 3 명이 같이 나가게 됩니다.

```
1
2
3
4
5
6
7
const boxes = [5, 1, 4, 6];
const output = paveBox(boxes);
console.log(output); // 3

const boxes2 = [1, 5, 7, 9];
const output2 = paveBox(boxes2);
console.log(output2); // 1
```

## 풀이

```jsx
function paveBox(boxes) {
    let answer = [];
    
    // boxes 배열이 0보다 클 때까지 반복합니다.
    while(boxes.length > 0){
        let finishIndex = boxes.findIndex(fn => boxes[0] < fn);
        
        if(finishIndex === -1){
            // 만약 찾지 못했다면 answer에 boxes 배열의 길이를 넣은 후, boxes 내부의 요소를 전부 삭제합니다.
            answer.push(boxes.length);
            boxes.splice(0, boxes.length);

        } else {
            // 만약 찾았다면, 해당 인덱스를 answer에 넣고, boxes에서 그만큼 제외합니다.
            answer.push(finishIndex);
            boxes.splice(0, finishIndex);
        }
    }

    // 결과물을 반환합니다.
    return Math.max(...answer);
}
```
