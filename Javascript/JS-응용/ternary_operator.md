# 삼항연산자

삼항연산자는 조건문을 확 줄여서 사용할 수 있는 연산자이다.
만약 주어진 숫자가 양수인지 음수인지 확인하는 함수가 있다면 아래코드와 같이 작성할 수 있다.

```
let a = 3;

if (a >= 0) {
  console.log("양수");
} else {
  console.log("음수");
}
```

간단한 조건식이지만 삼항연산자를 활용하면 더욱더 코드길이가 짧아진다.

```
let a = 3;

a >= 0 ? console.log("양수") : console.log("음수");
```

코드를 하나하나 살펴보면 a >= 0 이 부분은 조건식이다.
? 다음에 오는 console.log("양수")는 참일때 수행할 문자 : 뒤에는 false일때 수행할문장이다. 즉
조건 ? true코드 : false코드 인 것이다. 하나 더 해보자.

```
let a = [1, 2, 3, 4];

a.length === 0 ? console.log("빈 배열입니다.") : console.log("빈 배열이 아닙니다.");

출력 : 빈 배열이 아닙니다.
```

그리고 console.log를 사용하지 않고 그냥 문자열로 변수나 상수에 저장시켜 사용 할 수도 있다.

```
let a = [];

const arrStatus = a.length === 0 ? "빈 배열입니다." : "빈 배열이 아닙니다.";

console.log(arrStatus);

출력 : 빈 배열입니다.
```

## 삼항연산자의 truthy & falsy

```
let a;
let b = [];

const resultA = a ? true : false;
const resultB = b ? true : false;

console.log(resultA); // false
console.log(resultB); // true
```

a는 undefined값을 가지고 있으니 false며 , b는 빈 배열이니 true이다.

## 삼항연산자의 중첩

삼항연산자도 else-if 처럼 중첩이 가능하다.
예를 들어 90점 이상 A+ / 50점 이상 B+ / 그 외 F로 출력해야 하는 함수를 만들어야 된다고 하면 아래와 같이 삼항연산자를 사용할 수 있다.

```
let score = 100;

score >= 90
  ? console.log("A+")
  : score >= 50
  ? console.log("B+")
  : console.log("F");
```

말 그대로 해석하자면 score가 90보다 크거나 같니 ? 참이면 A+ 거짓이면 score가 50보다 크거나 같이 ? 참이면 B+ 거짓이면 F가 된다.
**_ 이렇게 삼항연산자도 중첩이 가능하지만 if문보다 훨씬 가독성이 떨어지게 된다. 이렇게 긴 코드를 작성할 때는 if를 사용하는게 좋다. _**
