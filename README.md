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

- **구현**
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

- **테스트**
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

## Queue
> FIFO(First In First Out) 컬렉션
- 새 원소는 뒤로 들어가서 앞으로 빠져나가는 구조이다.
- 따라서 마지막에 추가된 원소는 큐의 뒤에서 가장 오래 대기해야 한다.
- 큐와 스택은 원소의 추가/삭제 원리만 다를 뿐 나머지는 동일하다. <br/><br/>
  ![img.png](assets/queue.png)

### 구현단계
- 구현해야할 메서드
  - enqueue(items): 큐의 뒤쪽에 원소(들)을 추가한다.
  - dequeue(): 큐의 첫 번째(맨 앞) 원소를 반환하고 삭제한다.
  - front(): 큐의 첫 번째 원소를 반환하되, 삭제하지 않는다. (스택의 peek 메소드와 비슷하다.)
  - isEmpty(): 큐가 비어있으면 true, 비어있지 않으면 false 반환
  - size(): 큐에 있는 원소의 개수를 반환한다. 배열의 length 프로퍼티와 동일

- **구현**
```javascript
class Queue {
  constructor() {
    this.items = [];
  }

  enqueue(...items) {
    this.items.push(...items);
  }

  dequeue() {
    return this.items.shift();
  }

  front() {
    return this.items.at(0);
  }

  isEmpty() {
    return !this.items.length;
  }

  size() {
    return this.items.length;
  }
}
```
- **테스트**
```javascript
const queue = new Queue();

console.log(queue.isEmpty()); // true

queue.enqueue("ha"); // queue: ["ha"]
queue.enqueue("jeong"); // queue: ["ha", "jeong"]
queue.enqueue("hun"); // queue: ["ha", "jeong", "hun"]

console.log(queue); // ["ha", "jeong", "hun"]
console.log(queue.size()); // 3
console.log(queue.isEmpty()); // false

queue.dequeue(); // "ha" 반환
queue.dequeue(); // "jeong" 반환
console.log(queue); // ["hun"]
```

## Linked List
> 데이터를 노드(Node) 형태로 저장하는 선형 데이터 구조
- 각 노드는 데이터를 포함하며 다음 노드를 가리키는 포인터(또는 참조)를 가지고 있다.
- 배열은 인덱스로 원소에 바로 접근할 수 있지만, 연결 리스트는 원소를 찾기위해 Head부터 순차적으로 접근해야 한다.
  <br/><br/>
  ![img.png](assets/linked-list.png)

### 구현단계
- 구현해야 할 헬퍼 클래스(Node)
  - element: 원소
  - next: 다음 원소에 대한 포인터
- 구현해야 할 변수
  - length: 연결리스트의 총 원소 개수
  - head: 연결이 시작되는 지점
- 구현해야할 메서드
  - append(element): 연결 리스트의 맨 끝에 원소를 추가한다.
  - insert(position, element): 해당 위치에 원소를 삽입한다.
  - removeAt(position): 해당 위치의 원소를 삭제한다.
  - remove(element): 해당 원소를 삭제한다.
  - indexOf(element): 해당 원소의 인덱스를 반환한다. 미존재 시 -1 반환
  - isEmpty(): 원소가 하나도 없으면 true, 있으면 false 반환
  - size(): 원소 개수를 반환. 배열의 length 프로퍼티와 동일
  - toString(): 원소가 Node에 담겨있기 때문에 원소의 값만을 출력하기 위해 toString 메소드 재정의


- **append(element) 구현** - 연결 리스트의 맨 끝에 원소를 추가한다.
```javascript
class Node {
  constructor(element) {
    this.element = element;
    this.next = null;
  }
}
class LinkedList {
  #length = 0;
  #head = null;

  append(element) {
    const node = new Node(element);

    if (this.#head === null) { // (1) 원소가 비었을 때
      this.#head = node;
    } else {
      let current = this.#head;

      // (2) next가 null일 때까지 반복 
      while (current.next) {
        current = current.next;
      }

      // (3) next를 방금 추가한 Node로 등록
      current.next = node;
    }

    // (4) 원소의 길이 1 추가
    this.#length += 1;
  }
}
```
- (1) - 원소가 비었을 경우에는, 추가하는 원소를 연결 리스트의 head로 등록한다.
- (2) - 연결 리스트에 원소가 이미 있는 경우, 가장 뒤에 추가해야 하기 때문에 Node의 next 포인트가 null인 꼬리(tail)지점까지 접근을 반복한다.
- (3) - current가 가장 뒤의 Node로 접근했기 때문에 next 포인트를 추가할려고 한 원소를 가리키게 한다.
- (4) - 연결리스트의 원소가 추가되었기 때문에 length 프로퍼티를 1 더한다.


- **insert(position, element) 구현** - 해당 위치에 원소를 삽입한다.
```javascript
class LinkedList {
  #length = 0;
  #head = null;

  // ...(생략)

  insert(position, element) {
    // (1) 삽입 위치가 범위를 벗어남
    if (position < 0 || position > this.#length) return false;

    const node = new Node(element)
    let previous = null;
    let current = this.#head;

    // (2) 첫 위치에 삽입 시
    if (position === 0) {
      node.next = current;
      this.#head = node;
      this.#length += 1;

      return true;
    }

    for (let i = 0; i < position; i++) {
      previous = current; // (3) 삽입 위치 이전 노드
      current = current.next // (4) 삽입 위치의 기존 노드
    }

    // (5)
    node.next = current;
    previous.next = node;

    this.#length += 1;

    return true;
  }
}
```
- (1) - 삽입 위치가 범위를 벗어날 경우 false 반환
- (2) - 첫 위치에 삽입 시, 우선 신규 Node의 next가 head를 바라보게 한 뒤에 head를 신규 Node로 옮긴다. (`head` -> `New Node` -> `Existing First Node`)
- (3) - poisition이 2라고 가정했을 때, 1번 Node 를 뜻한다.
- (4) - poisition이 2라고 가정했을 때, 기존 연결 리스트의 2번 Node 를 뜻한다.
- (5) - 신규 Node의 next가 기존 2번 Node를 바라보게 하고, 1번 Node의 next가 신규 Node를 바라보게 한다. (`head` -> `index {1} Node` -> `index {2} New Node` -> `Existing index {2} Node`)


- **removeAt(position) 구현** - 해당 위치의 원소를 삭제한다.
```javascript
class LinkedList {
  #length = 0;
  #head = null;

  // ...(생략)

  removeAt(position) {
    // (1) 삭제 위치가 범위를 벗어남
    if (position < 0 || position >= this.#length) return null;

    const node = new Node(element)
    let previous = null;
    let current = this.#head;

    // (2) 첫 위치 원소 삭제 시
    if (position === 0) {
      this.#head = current.next;
      this.#length -= 1;

      return current.element;
    }

    for (let i = 0; i < position; i++) {
      previous = current; // (3) 삭제할 노드의 이전 노드
      current = current.next // (4) 삭제할 노드
    }

    // (5)
    previous.next = current.next;

    this.#length -= 1;

    return current.element;
  }
}
```
- (1) - 삭제 위치가 범위를 벗어날 경우 null 반환
- (2) - 첫 위치의 Node 삭제 시, head가 두번째 Node를 바라보게 한다. (첫번째 Node를 참조하는 곳이 없어 GC 수행)
- (3) - poisition이 2라고 가정했을 때, 1번 Node 를 뜻한다.
- (4) - poisition이 2라고 가정했을 때, 기존 연결 리스트의 2번 Node 를 뜻한다.
- (5) - 1번 Node의 next가 2번 Node의 next인 3번 Node를 바라보게 하면 2번 Node는 GC 수행되며 제거

![img.png](assets/linked-list(removeAt).png)

- **indexOf(element) 구현** - 해당 원소의 인덱스를 반환한다. 미존재 시 -1 반환
```javascript
class LinkedList {
  #length = 0;
  #head = null;

  // ...(생략)

  indexOf(element) {
    let current = this.#head;
    let index = -1;

    // (1)
    while (current) {
      index += 1; // (2)

      if (current.element === element) {
        return index; // (3)
      }

      current = current.next;
    }

    // (4)
    return -1;
  }
}
```
- (1) - current는 현재 위치로 전부 순회해도 없는 경우 current는 null을 가지며 반복문이 종료된다.
- (2) - 반복문을 수행할때마다 index를 1개씩 증가한다
- (3) - 인자로 받은 원소와 일치한 원소가 있으면 즉시 index를 반환한다.
- (4) - 반복문이 모두 수행됐음에도 발견된 원소가 없으므로 -1을 반환한다.


- **toString() 구현** - 원소가 Node에 담겨있기 때문에 원소의 값만을 출력하기 위해 toString 메소드 재정의
```javascript
class LinkedList {
  #length = 0;
  #head = null;

  // ...(생략)

  toString() {
    let current = this.#head;
    let string = '';

    // (1)
    while (current) {
      string += current.element; // (2)
      current = current.next;
    }

    return string;
  }
}
```
- (1) - current는 현재 위치로 전부 순회해도 없는 경우 current는 null을 가지며 반복문이 종료된다.
- (2) - node의 element 값을 string 변수에 이어 붙인다.


- **remove(element) 구현** - 해당 원소를 삭제한다.
```javascript
class LinkedList {
  #length = 0;
  #head = null;

  // ...(생략)

  remove(element) {
    const index = this.indexOf(element); // (1)
    return this.removeAt(index); // (2)
  }
}
```
- (1) - 먼저 구현한 indexOf 메소드를 이용해서 해당 원소의 index를 찾는다.
- (2) - 먼저 구현한 removeAt 메소드에 (1)에서 찾은 index를 이용하여 원소를 삭제하고 반환한다.


- **isEmpty() 구현** - 원소가 하나도 없으면 true, 있으면 false 반환
```javascript
class LinkedList {
  #length = 0;
  #head = null;

  // ...(생략)

  isEmpty() {
    return !this.#length; // (1)
  }
}
```
- (1) - length가 0이면 true, 0이 아니면 false를 반환한다.


- **size() 구현** - 원소 개수를 반환. 배열의 length 프로퍼티와 동일
```javascript
class LinkedList {
  #length = 0;
  #head = null;

  // ...(생략)

  size() {
    return this.#length;
  }
}
```

## Doubly Linked List
> 이중 연결 리스트는 이전(prev) Node, 다음(next) Node 2개의 연결정보를 갖고있다.
- 연결 리스트는 next Node로만 접근이 가능했지만, 이중 연결 리스트는 prev Node에도 접근 할 수 있다.
- 첫 번째 원소를 가리키는 Head 프로퍼티 외에도 마지막 원소를 가리키는 Tail 프로퍼티가 추가됐다.
- 구조는 다음 그림과 같다.
  <br/><br/>
  ![img.png](assets/doubly-linked-list.png)

### 구현단계
- 구현해야 할 헬퍼 클래스(Node)
  - element: 원소
  - prev: 이전 원소에 대한 포인터
  - next: 다음 원소에 대한 포인터
- 구현해야 할 변수
  - length: 연결리스트의 총 원소 개수
  - head: 연결이 시작되는 지점
  - tail: 연결이 끝나는 지점
- 구현해야할 주요 메서드 ()
  - insert(position, element): 해당 위치에 원소를 삽입한다.
  - removeAt(position): 해당 위치의 원소를 삭제한다.


- **insert(position, element) 구현** - 해당 위치에 원소를 삽입한다.
```javascript
class Node {
  constructor(element) {
    this.element = element;
    this.prev = null;
    this.next = null;
  }
}
class DobulyLinkedList {
  #length = 0;
  #head = null;
  #tail = null;

  insert(position, element) {
    if (position < 0 || position > length) {
      return false;
    }

    const node = new Node(element);
    let current = this.#head;
    let previous = null;
    let index = 0;

    // (1)
    if (position === 0) {
      if (!this.#head) {
        this.#head = node;
        this.#tail = node;
      } else {
        current.prev = node;
        node.next = current;
        this.#head = node;
      }

      this.#length += 1;
      return true;
    }

    // (2)
    if (position === this.#length) {
      this.#tail.next = node;
      node.prev = this.#tail;
      this.#tail = node;

      this.#length += 1;
      return true;
    }

    // (3)
    while (index++ < position) {
      previous = current;
      current = current.next;
    }

    // (4)
    previous.next = node;
    node.prev = previous;
    node.next = current;
    current.prev = node;

    this.#length += 1;

    return true;
  }
}
```
- (1) - 첫 번째(index: 0)위치에 삽입 하는 경우,
  - head가 null인 경우 리스트가 빈 상태이므로 head, tail을 모두 추가할 원소를 바라보게 한다.
  - 리스트가 비어있지 않은 경우에는, current(현재 head)의 이전(prev)원소를 추가할 원소로 바라보게하고, 추가할 원소의 다음(next) 원소를 current를 바라보게 하여 연결을 완료한다.
- (2) - 마지막위치에 삽입 하는 경우, tail.next를 신규원소로 설정하고, 신규 원소의 prev를 기존 tail 원소로 바라보게하여 연결을 완료한다. 그리고 마지막으로 tail 프로퍼티를 신규 원소로 다시 재설정해준다.
- (3) - 임의의 위치에 삽입 하는 경우로, 기존 연결 리스트처럼 해당 위치까지 previous, current를 이동시킨다.
  - 여기서 추가로 length 프로퍼티의 값과 삽입할 위치를 비교하여 tail에 가까운 경우에는 뒤에서 순회하면 성능적인 이점을 가질 수 있다.
- (4) - 해당 위치로 이동이 완료된 뒤, previous, node, current 간의 `prev`, `next`를 확실히 연결하여 연결이 끊기지 않도록 한다.


- **removeAt(position) 구현** - 해당 위치의 원소를 삭제한다.
```javascript
class DobulyLinkedList {
  #length = 0;
  #head = null;
  #tail = null;

  removeAt(position) {
    if (position < 0 || position > length) {
      return null;
    }

    let current = this.#head;
    let previous = null;
    let index = 0;

    // (1)
    if (position === 0) {
      if (!this.#head) {
        return null;
      }

      this.#head = current.next;

      if (this.#length === 1) {
        this.#tail = null;
      } else {
        this.#head.prev = null;
      }

      this.#length -= 1;

      return current.element;
    }

    // (2)
    if (position === this.#length - 1) {
      current = this.#tail;
      this.#tail = current.prev;
      this.#tail.next = null;

      this.#length -= 1;

      return current.element;
    }

    // (3)
    while (index++ < position) {
      previous = current;
      current = current.next;
    }

    // (4)
    previous.next = current.next;
    current.next.prev = previous;

    this.#length -= 1;

    return current.element;
  }
}
```
- (1) - 첫 번째(index: 0)위치를 삭제 하는 경우, head를 두번 째 원소를 바라보게한다.
  - 만약, 리스트의 개수가 1개였다면 tail도 null 처리를 해준다.
  - 리스트가 2개 이상이였다면 tail은 유지하고 head의 prev 값을 null로 설정하여 첫번째 원소와의 연결을 끊는다.
- (2) - 마지막위치를 삭제 하는 경우, tail을 마지막에서 2번째 원소로 바라보게 하고 tail.next를 null로 설정하여 마지막 원소와의 연결을 끊는다.
- (3) - 임의의 위치를 삭제 하는 경우로, 해당 위치까지 previous, current를 이동시킨다.
  - insert(추가) 메소드와 마찬가지로 length 프로퍼티의 값과 삭제할 위치를 비교하여 tail에 가까운 경우에는 뒤에서 순회하면 성능적인 이점을 가질 수 있다.
- (4) - 해당 위치로 이동이 완료된 뒤, 삭제할 원소위치의 이전,이후 원소 간의 `next`, `prev`를 확실히 연결하여 삭제할 원소와의 연결을 끊는다.

## Set
> 집합(Set)은 데이터를 중복 없이 저장하고 순서에 의존하지 않는 비선형 데이터 구조
- 집합 자료 구조는 수학에서의 유한집합(finite set)의 개념을 컴퓨터 과학에 적용한 것이다.
- 정렬 개념이 없는, 원소가 중복되지 않는 배열이라고 볼 수 있다.
- 집합은 합집합, 교집합, 차집합 같은 수학적 연산이 가능하다.

### 구현단계
- 구현해야할 메서드
  - add(value): 원소를 추가한다.
  - remove(value): 원소를 삭제한다.
  - has(value): 어떤 원소가 집합에 포함되어 있는지 여부를 true/false로 반환한다.
  - clear(): 모든 원소를 삭제한다.
  - size(): 원소의 개수를 반환한다. 배열의 length 프로퍼티와 동일
  - values(): 집합의 모든 원소를 배열 형태로 반환한다.

- **구현**
```javascript
class Set {
  constructor() {
    this.items = {};
  }

  has(value) {
    return this.items.hasOwnProperty(value);
  }

  add(value) {
    if (this.has(value)) {
      return false;
    }

    this.items[value] = value;
    return true;
  }

  remove(value) {
    if (this.has(value)) {
      delete this.items[value];
      return true;
    }

    return false;
  }

  clear() {
    this.items = {};
  }

  size() {
    return Object.keys(this.items).length;
  }

  values() {
    return Object.keys(this.items);
  }
}
```

- **테스트**
```javascript
const set = new Set();

set.add(1);
console.log(set.values()); // ["1"]
console.log(set.has(1)); // true
console.log(set.size()); // 1

set.add(2);
console.log(set.values()); // ["1", "2"]
console.log(set.has(2)); // true
console.log(set.size()); // 2

set.remove(1);
console.log(set.values()); // ["2"]

set.remove(2);
console.log(set.values()); // []
```

### 집합 연산
> 집합에서 사용 가능한 연산은 네 가지가 있다.
- 합집합 (Union): 두 집합 중 어느 한쪽이라도 포함된 원소로 구성된 집합
- 교집합 (Intersection): 두 집합 모두 포함되어 있는 원소로 구성된 집합
- 차집합 (Difference): 첫 번째 집합에는 있지만 두 번째 집합에는 없는 원소로 구성된 집합
- 부분집합 (Subset): 어떤 집합이 다른 집합의 일부인 집합


### 1. **합집합 (Union)**
- **기호**: ( A ∪ B )
- **의미**: ( A )와 ( B )의 모든 원소를 포함하는 집합. 중복은 허용되지 않음.
- **예**: ( A = {1, 2, 3}, B = {3, 4, 5} )일 때, ( A ∪ B = {1, 2, 3, 4, 5} )
- **구현**
```javascript
class Set {
  // ...(생략)

  union(otherSet) {
    const unionSet = new Set(); // (1)
    const unionValues = [...this.values(), ...otherSet.values()]; // (2)

    for (const value of unionValues) {
      unionSet.add(value); // (3)
    }

    return unionSet;
  }
}
```
- (1) - 합집합으로 만들 새로운 set을 생성한다.
- (2) - 현재 집합(A)의 모든 원소들과 다른 집합(B)의 모든 원소의 value를 한개의 배열로 합친다.
- (3) - 원소를 순회하면서 unionSet(합집합)에 추가해준다.


- **테스트**
```javascript
const setA = new Set();

setA.add(1);
setA.add(2);
setA.add(3);

const setB = new Set();

setB.add(1);
setB.add(2);
setB.add(3);
setB.add(4);
setB.add(5);
setB.add(6);

const unionSet = setA.union(setB);
console.log(unionSet.values()); // ["1", "2", "3", "4", "5", "6"]
```

---

### 2. **교집합 (Intersection)**
- **기호**: ( A ∩ B )
- **의미**: ( A )와 ( B )에 **공통으로 포함된 원소들**의 집합.
- **예**: ( A = {1, 2, 3}, B = {3, 4, 5} )일 때, ( A ∩ B = {3} ).
- **구현**
```javascript
class Set {
  // ...(생략)

  intersection(otherSet) {
    const intersectionSet = new Set(); // (1)
    const otherSetValues = otherSet.values();

    for (let i = 0; i < otherSetValues.length; i++) {
      if (this.has(otherSetValues[i])) { // (2)
        intersectionSet.add(otherSetValues[i]); // (3)
      }
    }

    return intersectionSet;
  }
}
```
- (1) - 교집합으로 만들 새로운 set을 생성한다.
- (2) - 현재 집합(A)에도 다른 집합(B)의 원소가 있는지 확인한다.
- (3) - 두 집합 모두 원소가 존재하면 교집합 set에 원소를 추가한다.


- **테스트**
```javascript
const setA = new Set();

setA.add(1);
setA.add(2);
setA.add(3);

const setB = new Set();

setB.add(3);
setB.add(4);
setB.add(5);
setB.add(6);

const intersectionSet = setA.intersection(setB);
console.log(intersectionSet.values()); // ["3"]
```

---

### 3. **차집합 (Difference)**
- **기호**: ( A - B ) 또는 ( A ∖ B )
- **의미**: ( A )에 속하면서 ( B )에는 속하지 않는 원소들의 집합.
- **예**: ( A = {1, 2, 3}, B = {3, 4, 5} )일 때, ( A - B = {1, 2} ).
- **구현**
```javascript
class Set {
  // ...(생략)

  difference(otherSet) {
    const differenceSet = new Set(); // (1)
    const values = this.values();

    for (let i = 0; i < values.length; i++) {
      if (!otherSet.has(values[i])) { // (2)
        differenceSet.add(values[i]); // (3)
      }
    }

    return differenceSet;
  }
}
```
- (1) - 차집합으로 만들 새로운 set을 생성한다.
- (2) - 순회중인 현재 집합(A)의 원소가 다른 집합(B)에는 없는지 확인한다.
- (3) - 현재 집합(A)에는 있지만 다른 집합(B)에는 존재하지 않으면 차집합 set에 원소를 추가한다.


- **테스트**
```javascript
const setA = new Set();

setA.add(1);
setA.add(2);
setA.add(3);

const setB = new Set();

setB.add(2);
setB.add(3);
setB.add(4);

const differenceSet = setA.difference(setB);
console.log(differenceSet.values()); // ["1"]
```

---

### 4. **부분집합 (Subset)**
- **기호**: ( A ⊆ B )
- **의미**: ( A )의 모든 원소가 ( B )에 포함되어 있을 때, ( A )는 ( B )의 부분집합이다.
- **기호 설명**:
  - ( A ⊆ B ): ( A )는 ( B )의 **부분집합** (같거나 작음).
  - ( A ⊂ B ): ( A )는 ( B )의 **진부분집합** (엄밀히 작음, 즉 ( A \neq B )).
- **예**: ( A = {1, 2}, B = {1, 2, 3} )일 때, ( A ⊆ B ).
- **구현**
```javascript
class Set {
  // ...(생략)

  subset(otherSet) {
    const subSet = new Set(); // (1)
    
    if (this.size() > otherSet.size()) { // (2)
        return false;
    }
    
    const values = this.values();

    for (let i = 0; i < values.length; i++) {
      if (!otherSet.has(values[i])) { // (3)
        return false;
      }
    }

    return true; // (4)
  }
}
```
- (1) - 부분집합으로 만들 새로운 set을 생성한다.
- (2) - 현재 집합(A)의 size가 다른 집합(B)의 size보다 크다면 이미 부분집합의 조건에 해당하지 않아 false를 반환한다.
- (3) - 현재 집합(A)에는 있지만 다른 집합(B)에는 존재하지 않으면 이 역시 부분집합 조건에 만족하지 않아 false를 반환한다.
- (4) - 순회가 끝나고 현재 집합(A)의 모든 원소가 다른 집합(B)에 포함되는 경우로, 부분집합 조건을 만족하여 true를 반환한다.

- **테스트**
```javascript
const setA = new Set();

setA.add(1);
setA.add(2);

const setB = new Set();

setB.add(1);
setB.add(2);
setB.add(3);

const setC = new Set();

setC.add(2);
setC.add(3);
setC.add(4);

console.log(setA.subset(setB)); // true
console.log(setA.subset(setC)); // false
```

---
