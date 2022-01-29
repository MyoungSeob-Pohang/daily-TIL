# 배열의 내장함수

배열은 많은 내장함수를 가지고 있다. 내장함수를 사용하면 더 깔끔하고 간결하게 코드를 작성할 수 있다.

## forEach

이전 문서에서 반복문(for)을 사용하여 배열을 순환하였다. 조금 더 쉽게 사용하기 위한 forEach를 소개한다.

```
const arr = [1, 2, 3, 4, 5];

arr.forEach((elm) => console.log(elm));
```

지금보면 forEach 안에 callBack 함수가 들어가 있다. 배열의 모든 요소를 한번씩 순회할 수 있도록 해준다. for문 보다 조금 더 짧고 간결해졌다.
ele에서 1, 2, 3, 4, 5를 순서대로 받아서 그것을 출력하고 있는 것이다. 만약 배열의 요소에 _ 2한 값을 알고 싶다면 , 아래와 같이 _ 2만 넣어주면 된다.

```
const arr = [1, 2, 3, 4, 5];

arr.forEach((elm) => console.log(elm * 2));
```

하지만 만약에 \*2 된 값을 새로운 배열로 생성하고 싶으면 어떻게 해야할까 ?
쉽게 생각하면 새로운 배열을 생성 후에 push 하면 되지 않을까 ? 맞다, 이상없이 작동한다.

```
const arr = [1, 2, 3, 4, 5];
const newArr = [];

arr.forEach((elm) => {
    newArr.push(elm * 2);
});

console.log(newArr);
```

## map

하지만 더 쉬운 방식이 존재한다. 바로 **_map_** 이다.
map은 원본 배열의 모든요소를 순회하면서 어떤 연산을 한 후 return으로 그 값을 받아오는게 가능하다.
즉 arr배열에 접근하여 1 _ 2를 리턴하고 2 _ 2를 리턴하고 3 \* 2를 차례대로 리턴하는 것이다.

```
const arr = [1, 2, 3, 4, 5];
const newArr = arr.map((elm) =>{
    return elm * 2;
});

console.log(newArr);
```

## includes

만약 특정한 값이 배열안에 존재하는지 확인하고 싶다면, forEach와 if문을 사용하여 확인할 수 있지만, 더 쉽게 includes라는 것을 사용할 수 있다.
아래 예제는 number라는 값이 arr배열에 존재하는지에 대한 값을 출력하는 코드이다.

```
const arr = [1, 2, 3, 4, 5];

let number = 3;

console.log(arr.includes(number)); // true
```

includes는 === 연산과 동일하게 type까지 같은지 확인한다. 만약 3이 아니라 "3" 이 전달되었다면 false를 반환하다.

## indexOf

includes와 동일하게 있는지 확인 후에 있으면 몇번째에 위치하고 있는지 알고 싶다면 indexOf를 사용한다.

```
const arr = [1, 2, 3, 4, 5];

let number = 3;

console.log(arr.indexOf(number)); // 2 출력
```

코드를 보면 number는 arr배열안에 index Number 2번째에 위치한다. 그래서 2를 출력하게 된다. 하지만 만약 존재하지 않는다면 -1을 반환하는데,
-1은 배열안에 그 숫자가 존재하지 않다는 뜻이된다.

## findIndex

findIndex는 배열의 첫 번째 요소에 대한 인덱스를 반환한다. 마찬가지로 존재하지 않으면 -1을 반환한다.

```
const arr = [
  { color: "red" },
  { color: "blue" },
  { color: "green" },
  { color: "black" }
];

console.log(arr.findIndex((elm) => elm.color === "black")); // 3출력
```

지금보면 findIndex에 콜백함수를 전달하고 있고, elm이 배열 객체에 하나씩 접근하면서 컬러가 black인 요소의 인덱스를 반환한다.
black는 인덱스 순서로 3번째에 위치하고 있음으로 3을 반환한다. 하지만 만약에 같은 요소가 존재한다면 첫번째 만나는 요소의 인덱스를 반환하고 종료된다.

```
const arr = [
  { color: "red" },
  { color: "blue" },
  { color: "green" },
  { color: "black" },
  { color: "red" },
  { color: "blue" }
];

console.log(arr.findIndex((elm) => elm.color === "blue")); // 1 출력
```

위와 같이 인덱스 5번에 위치한 blue는 무시된다. 인덱스 위치 1번에 위치한 blue를 만나서 종료되었기 떄문이다.
findIndex로 결국 반환받을 수 있는 값은 index 이다. 그래서 찾고자 하는 요소에 직접적으로 접근을 하고 싶다라고 한다면, 인덱스를 활용해서 접근할 수 있다.

```
const arr = [
  { color: "red" },
  { color: "blue" },
  { color: "green" },
  { color: "black" },
  { color: "red" },
  { color: "blue" }
];

const idx = arr.findIndex((elm) => elm.color === "blue");
console.log(arr[idx]); // {color: "blue"}
```

idx에는 결국 blue의 인덱스 값인 1이 저장이 될 것이고 아래에서 arr[1]을 출력하고 있다. 그래서 {color: "blue"}의 출력이 되는것을 확인할 수 있다.

## find

findIndex에서 우리는 인덱스 값을 찾을 수 있었다. 하지만 요소에 직접 접근하려고 하면 findIndex보다 find가 더 쉽고 간결하다.
find는 요소 자체를 반환하고 findIndex는 인덱스를 반환한다.

```
const arr = [
  { color: "red" },
  { color: "blue" },
  { color: "green" },
  { color: "black" },
  { color: "blue" },
];

const element = arr.find((elm) => elm.color === "blue");
console.log(element);
```

## filter

전달한 콜백함수가 trur를 반환하는 모든요소를 배열로 반환한다. 말 그대로 필터링의 역할이다.

```
const arr = [
  { num: 1, color: "red" },
  { num: 2, color: "blue" },
  { num: 3, color: "green" },
  { num: 4, color: "black" },
  { num: 5, color: "blue" }
];

let filtering = arr.filter((elm) => elm.color === "blue");

console.log(filtering);

출력 : (2) [Object, Object]

        0: Object
        num: 2
        color: "blue"

        1: Object
        num: 5
        color: "blue"
```

위 코드를 보면 color: blue값을 가진 요소가 2개 존재한다. 그 모두를 filter를 사용하여 배열로 반환 받은것을 알 수 있다.

## slice

```
const arr = [
  { num: 1, color: "red" },
  { num: 2, color: "blue" },
  { num: 3, color: "green" },
  { num: 4, color: "black" },
  { num: 5, color: "blue" }
];
```

위 arr에서 만약 num: 3까지만 자르고 싶다면 어떻게 해야할까 ? slice를 사용하면 된다.
조각낸다 자른다의 뜻을 가진 slice를 사용하면 내가 원하는 만큼 잘라서 배열로 반환받을 수 있다.

```
const arr = [
  { num: 1, color: "red" },
  { num: 2, color: "blue" },
  { num: 3, color: "green" },
  { num: 4, color: "black" },
  { num: 5, color: "blue" }
];

console.log(arr.slice());
console.log(arr.slice(0, 2));

출력 :  (5) [Object, Object, Object, Object, Object]
        0: Object
        1: Object
        2: Object
        3: Object
        4: Object

        (2) [Object, Object]
        0: Object
        1: Object
```

arr.slice()의 요소를 아무것도 주지 않으면 모두 자르기 때문에 똑같은 결과가 출력되지만 첫번째 인자(begin), 두번째인자(end)를 넣어주게 되면,
begin 부터 end 앞에 요소 까지 자른다. 그래서 console.log(arr.slice(0, 2)); 이 코드는 arr배열의 0번째 인덱스 부터 2번째 인덱스 앞에 까지 즉 1번 인덱스 까지 잘라라 라는 뜻이된다. 그래서 3개의 요소가 출력이 된게 아니라 2개의 요소 까지 출력이 된것이다.

## concat

자르는게 아니라 붙이고 싶으면 concat메소드를 사용한다.

```
const arr1 = [
  { num: 1, color: "red" },
  { num: 2, color: "blue" },
  { num: 3, color: "green" }
];

const arr2 = [
  { num: 4, color: "black" },
  { num: 5, color: "blue" }
];

console.log(arr1.concat(arr2));

출력 : (5) [Object, Object, Object, Object, Object]
        0: Object
        num: 1
        color: "red"
        1: Object
        num: 2
        color: "blue"
        2: Object
        num: 3
        color: "green"
        3: Object
        num: 4
        color: "black"
        4: Object
        num: 5
        color: "blue"
```

arr1과 arr2가 붙어서 5개 모두가 출력되는 것을 볼 수 있다.

## sort

배열의 정렬이다. 사전순으로 정렬하고 싶을 때 sort를 사용한다. 정렬 후 배열을 반환하는게 아니라 원본배열 자체를 정렬한다.

```
let chars = ["다", "나", "가"];

chars.sort();

console.log(chars);

출력 : (3) ["가", "나", "다"]
```

하지만 숫자를 정렬하고 싶다면 똑같이 사용하면 될까 ?

```
let numbers = [0, 1, 3, 2, 10, 30, 20];
numbers.sort();
console.log(numbers);

출력 : (7) [0, 1, 10, 2, 20, 3, 30]
```

위 코드의 순서를 보면 정렬이 조금 이상하다. sort는 문자열 기준으로 정렬하지 숫자를 기준으로 정렬하는게 아니다. 즉 사전 순으로 정렬하기 때문에 1 다음에 10, 2다음에 20이 오는 것이다. 이 코드를 정상적으로 바꿀려고 하면 sort메소드에 인자로 비교함수를 직접만들어서 넣어주면 된다.

```
let numbers = [0, 1, 3, 2, 10, 30, 20];

// 두개의 값을 비교해서 세가지 중 하나의 결과를 내놔야한다.
const compare = (a, b) => {
  if (a > b) {
    // 크다
    return 1;
  }
  if (a < b) {
    // 작다
    return -1;
  }
  // 같다
  return 0;
};

numbers.sort(compare);

console.log(numbers);

출력 : (7) [0, 1, 2, 3, 10, 20, 30]
        0: 0
        1: 1
        2: 2
        3: 3
        4: 10
        5: 20
        6: 30
```

a, b 두가지 값을 받을 때 1을 반환한다는 것은 a가 b보다 뒤에 있어야 한다는 뜻이고, -1은 a가 비보다 앞에 있어야 한다는 뜻이다.
즉 음수가 나오면 a가 앞으로 가고 양수가 나오면 b가 앞으로 간다. 같을 때 반환되는 0은 자리를 바꾸지 않게 된다.
결국은 오름차순 배열이 완료된다. 내림차순으로 변경하고 싶으면 retunr 1을 -1로 -1을 1로 바꾸어 주면 된다.

## join

join은 배열의 모든 문자열을 합쳐주는 메소드 이다. 코드로 보자.

```
const arr = ["moonsbi", "님", "안녕하세요", "만나서", "반갑습니다."];
console.log(arr[0] + arr[1] + arr[2] + arr[3] + arr[4]);

출력 : moonsbi님안녕하세요만나서반갑습니다.
```

이렇게 비효율적이고 손가락 아픈 코드를 작성하기 싫기 때문에 join을 사용한다.

```
const arr = ["moonsbi", "님", "안녕하세요", "만나서", "반갑습니다."];
console.log(arr.join());

출력 : moonsbi,님,안녕하세요,만나서,반갑습니다.
```

join()의 인자로 들어가는 값이 구분자가 된다. 만약 위 출력에서 ,가 보기싫다면 arr.join(' '); 이렇게 공백을 넣어주면 된다.
그럼 출력값으로 moonsbi 님 안녕하세요 만나서 반갑습니다. 가 출력이 된다.

더 많은 내장함수 : Link: [배열 내장함수][https://learnjs.vlpt.us/basics/09-array-functions.html]
