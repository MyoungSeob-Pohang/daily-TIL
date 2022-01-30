# 비 구조화 할당

배열과 객체를 더 우아하고 멋잇게 사용하는 비구조화 할당에 대해서 알아보자.

간단한 배열을 만들어서 그 배열의 값을 각 변수에 해당하는 코드를 아래와 같이 작성하였다.

## 배열의 비 구조화 할당

```
let arr = ["one", "two", "three"];

let one = arr[0];
let two = arr[1];
let three = arr[2];

console.log(one, two, three);
```

지금보면 arr이라는 변수가 계속 사용되는 것을 볼 수 있다. 이것을 획기적으로 1줄만에 줄 일수 있는데, 아래 코드와 같다.

```
let arr = ["one", "two", "three"];
let [one, two, three] = arr;

console.log(one, two, three);
```

이렇게 코드를 작성하게 되면 arr이라는 배열이 변수 one에 "one" , two에 "two, three에 "three"가 각각 저장된다.
이렇게 대괄호를 이용해서 배열의 값을 순서대로 할당받아 사용할 수 있게 하는 것을 배열의 비 구조화 할당이라고 한다.
위 코드를 더 줄일 수도 있는데 arr 자리에 그냥 배열이 존재하면 된다.

```
let [one, two, three] = ["one", "two", "three"];

console.log(one, two, three);
```

그런데 만약 변수가 하나 더 선언되면 어떻게 될까 ? 당연하게도 undefined가 할당된다.

```
let [one, two, three, four] = ["one", "two", "three"];

console.log(one, two, three, four);  // one two three undefined
```

undefined값이 할당되면 안된다고 할때에는 four에 기본값을 설정해 줄 수도 있다.

```
let [one, two, three, four = "four"] = ["one", "two", "three", "4"];

console.log(one, two, three, four);   // one two three 4
```

지금 코드를 보면 four에 four이라는 기본값을 주었지만 "4"라는 값이 있기 떄문에 기본값을 제외하고 4가 출력되는 것을 볼 수 있다.
만약 "4" 가 없었더라면 출력은 one two three four 가 출력되었을 것이다.

## SWAP

보통 각 변수의 값을 서로 바꿀려고 하면 1개의 변수가 더 필요하다 .

```
let a = 10;
let b = 20;
```

위 코드에서 a에 b의 값을 넣고 b에 a의 값을 넣고 싶으면 어떻게 할까 ? 보통은 새로운 변수를 선언하여 SWAP을 하게 된다.

```
let a = 10;
let b = 20;
let temp = a;

a = b;
b = temp;

console.log(a, b); // 20 10
```

하지만 비구조화할당을 사용하게 되면 더 쉽게 가능한데 아래와 같다.

```
let a = 10;
let b = 20;

[a, b] = [b, a];
console.log(a, b); // 20 10
```

그대로 해석하자만 b와 a의 값이 담긴 배열을 만들어서 이것을 a와 b에 비구조화할당을 통해 0번 인덱스 1번 인덱스에 대입한 것이다.
즉 [a, b] = [20, 10]이 각각 할당됨으로 a = 20, b = 10이 되는 것이다.

## 객체의 비구조화 할당

객체도 배열과 사용방법이 아주 유사하다. 대신 객체는 key값을 기준으로 비 구조화 할당이 이루어지기 때문에 변수명이 key와 일치해야 한다.
대신 순서는 상관이없다. 순서가 아니라 key값을 기준으로 비 구조화 할당이 이루어지기 때문이다.

```
let object = {
  one: "one",
  two: "two",
  three: "three",
  name: "moonsbi",
};

// 기존 방법
// let one = object.one;
// let two = object.two;
// let three = object.three;
// let name = object.name;

// 비 구조화 할당 key값 기준으로 할당
// key값과 변수명이 일치해야 한다, 대신 순서는 상관없다.

let { name, two, one, three } = object;

console.log(one, two, three, name); // one two three moonsbi
```

key값과 변수명이 일치해야 한다는 강제성이 존재한다고 위에서 언급하였지만 이 부분은 극복할 수 있다. 만약 내가 name이라는 key 값에 접근하는데 다른 변수명을 사용하고 싶다면,
: 을 사용하면 된다.

```
let object = {
  one: "one",
  two: "two",
  three: "three",
  name: "moonsbi",
};

let { name: myName, two, one, three } = object;

console.log(one, two, three, myName); // one two three moonsbi
```

지금보면 변수명 name 뒤에 :myName 이라는 새로운 변수명을 달아주었다. 그래서 myName으로 접근이 가능하다.
myName으로 변수명을 정해주었기 떄문에 기존의 name으로는 접근이 불가능하다.
그리고 배열과 동일하게 존재하지 않는 key값에 접근할 경우 undefined가 출력이되고, 배열과 동일하게 기본값을 할당 할 수있다.
