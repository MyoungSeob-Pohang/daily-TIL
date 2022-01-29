# 자료형

자료형이랑 자료의 성질에 따라서 값을 분류한 것이다.
자료형의 종류로는 원시타입(primitive Type)과 비원시타입(non primitive Type)으로 나뉜다.

## 원시타입(primitive Type)

한번에 하나의 값 만 가질 수 있고 하나의 고정된 저장 공간을 이용한다.

1. Number : 1, 2, 30, 1001 처럼 숫자
2. String : "Moonsbu", "age", "name" 과 같은 문자
3. Boolean : True , False 와 같은 논리
4. Undefined : 정의되지 않은 값
5. Null : 값이 없음

**null과 Undefined의 차이는 null은 null이라는 값이 주어진 것이고 Undefined는 그냥 아무것도 하지않은 상태 \_**

```
let age = 25; // Number
let inf = Infinity;// Number 무한대
let inf = -Infinity;// Number - 무한대
let nan = NaN;// Number 숫자가 아님

let name = 'Moonsbi'; // String '', "", `` 안에 입력 / `` 안에는 ${age}와 같이 변수값을 가지고 올 수 있다
let handsome = false; // Boolean / 참이나 거짓인 값
let money; // Undefined, 선언 후 초기화 하지않으면 Undefined가 지정된다.
let Kkondae = null; // Null, 아무것도 없다
```

## 형변환

```
let numberA = 10;
let numberB = "2";

console.log(numberA + numberB);
console.log(numberA * numberB);
```

숫자와 문자를 더한다는 것은 말이 안되는 값이다. 하지만 자바스크립트는 숫자 10을 자동으로 문자 "10"으로 변환 시켜준다.
그래서 출력값은 102가 나온다. 문자인 10과 2를 붙여서 102가 된 것이다.
하지만 숫자 10과 문자 "2"를 곱하게 된다면 문자 "2"가 숫자 2로 변환되어 20이라는 숫자가 출력된다.
이렇게 자바스크립트는 적절하게 형을 자동으로 변환해주게 된다 그리고 이것을 묵시적형변환이라고 한다.

하지만 여기서 궁금증이 생긴다. 만약 console.log(numberA + numberB); 이 값을 102가 아닌 12로 만들고 싶으면 어떻게 해야할까 ?
이럴때 사용자가 직접 형변환을 하는 명시적형변환을 사용한다.

```
let numberA = 10;
let numberB = "2";

console.log(numberA + parseInt(numberB));
```

parseInt()라는 함수는 문자열을 받아서 숫자로 돌려준다. 이것을 사용하여 numberB 라는 문자가 숫자 2로 변환 된 것이다.
이렇게 하면 12라는 숫자를 얻을 수 있다.

## 비원시타입(non primitive Type)

한번에 여러개의 값을 가질 수 있으며 여러개의 고정되지 않은 동적 공간을 사용한다. 차후 다른 md에서 더 자세하게 다룰 예정입니다.

1. Object : key : value 값으로 여러 속성을 하나의 변수에 저장할 수 있도록 해주는 타입
2. Array : key를 사용해 식별할 수 있는 값을 담을 수 있는 자료구조
3. Function : 작업을 수행하거나 값을 계산하는 등 특별한 목적의 작업을 수행하도록 하는 블록

```
let object = {
    name: 'moonsbi',
    age: '50',
}

let array = ['moonsbi', 50];

function Add(x, y){
    console.log(x + b);
}
```

차례대로 객체, 배열, 함수이며 객체는 key : value 값으로 지정된 값인 걸 볼 수 있고, array는 []안에 여러 값이 들어 간 것을 볼 수 있다.
function Add는 x와 y값을 받아서 그 수를 더한 값을 출력하고 있다.
