# 배열

비원시타입의 마지막 배열이다. 배열은 순서있는 요소들의 집합이라고 정의할 수 있다.

## 배열의 생성방법

객체와 동일하게 생성자로 생성하는 방법과 배열리터럴 방법이 있다. 리터럴 방법은 객체와 다르게 [] 대괄호를 사용한다.

```
let arr = new Array(); // 생성자
let arr1 = []; // 리터럴
```

그리고 배열은 객체와 동일하게 무슨 자료형이든 가질 수 있다.

```
let arr = [1, "2", true, null, undefined, {}, [], function() {}];
console.log(arr);

결과 : (8) [1, "2", true, null, undefined, Object, Array(0), ƒ ()]
        0: 1
        1: "2"
        2: true
        3: null
        4: undefined
        5: Object
        6: Array(0)
        7: ƒ () {}
```

위 결과를 보면 모든 자료형이 저장된 것을 알 수 있는데 이상한 점이 있다. 8개를 삽입했는데 앞에 번호가 7번까지 있는 것이다.
이것은 배열은 index numbering을 사용하기 때문에 첫번째 요소가 0번이 된다는 점이다. 0부터 사용하는 이유는 아래에 링크를 남겨두었다.  
 Link: [0부터 시작하는 이유와 마지막 수를 인덱스로 포함하지 않는 이유][https://nanite.tistory.com/56]

## 배열요소에 접근

배열은 객체와 다르게 key값이 존재하지 않는다. 대신에 index 번호로 접근을 하는데 아래와 같다.

```
let arr = [1, 2, 3, 4, 5];

console.log(arr[0]);    // 1
console.log(arr[1]);    // 2
console.log(arr[2]);    // 3
console.log(arr[3]);    // 4
console.log(arr[4]);    // 5
console.log(arr[5]);    // undefined
```

## 배열의 요소 추가

배열의 어느 부분에 추가를 할 건지에 따라 키워드가 달라진다.
push : 맨 끝에 추가
unshift : 맨 앞에 추가
splice : 내가 원하는 위치에 하나 이상의 요소 추가

```
let arr = ['a', 'b', 'c'];

// arr = ['a', 'b', 'c', 'd']
arr.push('d'); // 배열의 끝에 요소를 추가
```

```
let arr = ['a', 'b', 'c'];

// arr = ['d', 'a', 'b', 'c']
arr.unshift('d'); // 배열의 앞쪽에 요소를 추가
```

```
let arr = ['a', 'b', 'c'];

// arr = ['a', 'b', 'd', 'c']
arr.splice(2, 0, 'd'); // index 2 ('c')의 위치에 추가

// arr = ['a', 'b', 'd', 'c', 'e', 'f']
arr.splice(4, 0, 'e', 'f'); // index 4의 위치에 2개를 추가
```

## 배열의 요소 삭제

pop : 마지막 요소 제거
shift : 첫번째 요소 제거
splice : 위치를 정해 하나 이상의 요소를 제거

```
let arr = ['a', 'b', 'c', 'e', 'f'];

// arr = ['a', 'b', 'c', 'e']
arr.pop(); // 배열의 마지막 요소를 제거

// arr = ['a', 'b', 'c']
let popped = arr.pop(); // 제거한 요소를 반환 받을 수 있음

// popped = 'e'
```

```
let arr = ['a', 'b', 'c', 'e', 'f'];

// arr = ['b', 'c', 'e', 'f']
arr.shift(); // 배열의 첫번째 요소를 제거

// arr = ['c', 'e', 'f']
let shifted = arr.shift(); // 제거한 요소를 반환 받을 수 있음

// shifted = 'b'
```

```
let arr = ['a', 'b', 'c', 'e', 'f'];

// arr = ['a', 'b', 'e', 'f']
arr.splice(2, 1); // index 2 부터 1개의 요소('c')를 제거

// arr = ['a', 'f']
arr.splice(1, 2); // index 1 부터 2개의 요소('b', 'e')를 제거

// arr = ['a']
removed = arr.splice(1, 1); // 제거한 요소를 반환 받을 수 있음

// removed = 'f'
```

```
let arr = ['a', 'b', 'c', 'e', 'f'];

// arr = ["a", undefined, "c", "e", "f"]
delete arr[1]; // delete로 배열을 삭제할 경우 요소는 그대로 존재하며 값만 삭제 됨
```

Link: [자바스크립트 배열 추가, 삭제 방법][https://gent.tistory.com/295]

## 배열의 길이

length 키워드로 배열의 전체 길이를 알 수 있다.

```
let arr = [1, 2, 3, 4, 5];

console.log(arr.length); // 5출력
```
