# Truthy & Falsy

Truthy : 참이 아니라 참 같은 값 / Falsy : 거짓이 아니라 거짓 같은 값

```
let a = "";

if (a) {
  console.log("True");
} else {
  console.log("False");
}
```

위 코드에 a를 출력하면 False가 나온다. a는 비교연산도 아니고 그냥 "" 빈문자를 가진 string형인데 왜 False가 나오는걸까 ? string 형은 모두 false가 나올까 ?

```
let a = "string";

if (a) {
  console.log("True");
} else {
  console.log("False");
}
```

위 코드에 a를 출력하면 True가 나온다. 빈 문자일때는 false가 나왓는데 string라는 문자를 넣으니 True가 반환되었다.
이렇게 자바스크립트의 조건식에는 참/거짓(boolean)을 넣지 않더라도 참이나 거짓으로 인식이 되는 속성이 있다.
자바스크립트는 자신의 특정한 기준으로 값을 참 또는 거짓으로 분리한다. 그럼 자신만의 특정한 기준이 무엇일까 ?
그럼 참(true)으로 분류하는 값은 어떤것들이 있을까 ?

1. {} 빈 객체
2. [] 빈 배열
3. 0을 제외 한 숫자값
4. 빈 문자열을 제외한 문자열

이러한 true가 아니여도 true로 판단하는 값이 바로 Truthy이다.

그럼 Falsy는 무엇이 있을까 ?

1. null
2. undefined
3. 0
4. 빈 문자열
5. NaN

## Truthy & Falsy 활용

```
const getName = (person) => {
  return person.name;
};

let person = { name: "moonsbi" };
const name = getName(person);

console.log(name);

출력 : moonsbi
```

위 함수는 객체를 매개변수로 받아서 객체안에 이름 프로퍼티를 반환 받는 함수이다. 하지만 let person 이 undefined으로 전달이 된다면 어떻게 될까 ?

```
const getName = (person) => {
  return person.name;
};

let person;
const name = getName(person);

console.log(name);

Error :TypeError: Cannot read properties of undefined (reading 'name')
```

undefined은 객체가 아님으로 .표기법으로 프로퍼티를 사용하여 접근이 불가능하다. 그래서 에러가 나게 된다.
이것을 해결하기위해 Falsy의 속성을 활용하여 코드를 짜게 된다면 조금 더 안전한 코드가 된다.

```
const getName = (person) => {
  if (!person) {
    return "객체가 아닙니다.";
  }
  return person.name;
};

let person = null;
const name = getName(person);

console.log(name);

출력 : 객체가 아닙니다.
```

조건문을 선언 후 Not을 사용하여 !person 이라고 선언하면 한번에 해결이 된다. 위에서 설명하였듯이 null이나 undefined는 Falsy의 속성을 가지고 있다.
그래서 null이든 undefined이든 not을 붙이게 되면 true가 반환된다. 즉 false not = true기 때문에 if문안에 객체가 아닙니다. 가 리턴된다.
