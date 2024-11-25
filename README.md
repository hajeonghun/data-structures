# data-structures
본 프로젝트는 자료구조를 다시 정리하는 시간을 가지기 위한 TIL의 일환입니다.

# Index
- [Array](#array)
- [Stack](#stack)
- [Queue](#queue)
- [DeQueue](#dequeue)
- [Linked List](#linked-list)
- [Set](#set)
- [Dictionary/Hash](#dictionary/hash)
- [Graph](#graph)

## Array
 > 동일한 데이터 타입의 값을 연속적으로 저장한 것.  
 
 월별 날씨의 평균 온도를 일반 변수와 배열로 저장했을때 아래와 같다.
 ```javascript
 const averageJan = 31;
 const averageFeb = 35.3;
 const averageMar = 42.4;
 const averageApr = 60.8;
 
 // 위 코드를 배열을 사용하여 아래와 같이 관리할 수 있다.
const averageTemp = [31, 35.3, 42.3, 60.8];
 ```
![img.png](assets/array.png)

### 원소 추가
- JavaScript 배열은 가변(mutable) 객체이다. 
```javascript
// 방법 1.
const numbers = [1, 2, 3, 4];
numbers[4] = 5;

// 방법 2.
numbers.push(5);
numbers.push(6, 7);
```

- 배열의 마지막이 아니라 첫번째에 원소를 삽입하고 싶다면?
  - 기존에 있던 원소를 전부 우측으로 한 칸 씩 이동시키고 첫번째 원소에 삽입해야한다.
```javascript
// 방법 1.
const arr = [10, 20, 30]; // 초기 배열
const newValue = 5;       // 삽입할 값

// 배열 요소를 뒤로 이동
for (let i=arr.length - 1; i >= 0; i--) {
    arr[i + 1] = arr[i];
}

// 첫 번째 인덱스에 값 삽입
arr[0] = newValue;

console.log(arr); // 결과: [5, 10, 20, 30]

// 방법 2.
arr.unshift(-2, -4); // 결과: [-2, -4, 5, 10, 20, 30]
```

### 원소 삭제
- 마지막 원소 삭제
```javascript
const numbers = [1, 2, 3, 4];

numbers.pop(); // 4 제거
```
- 첫번째 원소 삭제
```javascript
// 방법 1.
const numbers = [1, 2, 3, 4];

for (let i=0; i < numbers.length; i ++) {
    numbers[i] = numbers[i+1];
}
```
이렇게 하면 첫번째 원소는 삭제되었지만 배열의 크기(length)는 그대로 4다.
그 말은, 배열의 값들이 덮어쓰였을 뿐 실제로 삭제한 것은 아니라는 뜻이다.
첫번째 원소를 삭제하며 길이도 제거하고 싶다면 `shift` 메소드를 이용하면 된다.

### 특정 위치에 원소 추가/삭제
- 3번째 위치에 원소 추가
```javascript
const numbers = [1, 2, 3, 4, 5, 6];

numbers.splice(3, 0, 30, 40); // 3번째 index에 30, 40 추가

console.log(numbers); // 결과: 1, 2, 3, 30, 40, 4, 5, 6
```
- 3번째 위치에서 원소 2개 삭제
```javascript
const numbers = [1, 2, 3, 4, 5, 6];

numbers.splice(3, 2); // 3번째 index 부터 2개 원소 삭제

console.log(numbers); // 결과: 1, 2, 3, 6
```

## Stack
> LIFO(Last In First Out) 컬렉션
- 스택은 항상 동일한 종단점에서 추가/삭제된다.
- 스택의 종단점은 Top or Base 2가지인데, 최근 자료는 Top에, 오래된 자료는 Base에 위치한다.
- JavaScript 엔진의 Call Stack이 스택 자료구조 기반이다.<br/><br/>
![img.png](assets/stack.png)

### 구현단계
- 구현해야할 메서드
  - push(items): Top에 원소(들)을 추가한다.
  - pop(): Top에 있는 원소를 반환하고 삭제한다.
  - peek(): Top에 있는 원소를 반환하되 삭제하지 않는다.
  - isEmpty(): 스택이 비어있으면 true, 비어있지 않으면 false 반환
  - clear(): 스택의 모든 원소를 삭제한다.
  - size(): 스택에 있는 원소의 개수를 반환한다. 배열의 length 프로퍼티와 동일  

- 구현
```javascript
class Stack {
    constructor() {
        this.items = [];
    }
    
    push(...items) {
        this.items.push(...items);
    }
    
    pop() {
        return this.items.pop();
    }
    
    peek() {
        return this.items.at(-1); // 최신 문법
    }
    
    isEmpty() {
        return !this.items.length;
    }
    
    clear() {
        this.items = [];
    }
    
    size() {
        return this.items.length;
    }
}
```  
  
- 테스트
```javascript
const stack = new Stack();

stack.push(1,2,3); // [1, 2, 3]
stack.push(4); // [1, 2, 3, 4]

stack.pop(); // 4
stack.pop(); // 3

console.log(stack); // 결과: [1, 2]

stack.size(); // 2
stack.isEmpty(); // false
```

- 간단한 문제 - 10진수를 2진수로 변환
```javascript
function divideBy2(decNumber) {
    const stack = new Stack();
    let tempDecNumber = decNumber;
    let result = ''; 
    
    while (tempDecNumber !== 0) { // O(log N)
        const division = Math.floor(tempDecNumber % 2);
        
        stack.push(division); // 나머지를 스택에 삽입
        
        tempDecNumber = Math.floor(tempDecNumber / 2);
    }
    
    /* 
     * 첫 번째 루프에서 스택에 삽입된 항목 수는 log₂(decNumber)에 비례하므로, 
     * 이 루프 역시 O(log₂(decNumber))입니다.
     */
    while (!stack.isEmpty()) { // O(log N)
      result += stack.pop(); // Top부터 꺼내어 2진수 변환
    }
    
    return result;
}
```
시간복잡도는 O(log N) 이다.
