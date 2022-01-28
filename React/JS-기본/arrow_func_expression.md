# 함수표현식

자료형문서에 Function에 대해 설명이 부족하여 추가한다.
사실은 함수도 자바스크립트 안에서 값 이다. 그래서 변수나 상수에 담아서 사용할 수 있다.

```
let hello = function () {
  return "Hello World!";
};

console.log(hello);

결과 :
        ƒ hello() {}
        <constructor>: "Function"
        name: "Function"
```

일단 hello라는 변수가 함수를 값으로 가지고 있다. 변수명인 hello라는 이름이 있기 때문에 변수나 상수에 대입할 때 함수는 함수명을 작성하지 않아도 문제가 없다.
hello라는 이름이 있기 때문이다. 그리고 결과값을 보면 함수랄 뜻으로 출력이 된다. 근데 hello가 담고 있는 함수는 return 으로 "Hello World!";를 내보내고 있다.
왜 "Hello World!"가 출력되지 않았는가 ? hello라는 변수가 곧 함수의 이름이기 때문이다. 그래서 함수호출하는 것 처럼 불러야 값이 지정된다.
이러한 형식을 **_함수표현식_** 이라고 한다

```
let hello = function () {
  return "Hello World!";
};

const helloText = hello();
console.log(hello()); // Hello World!
console.log(helloText); // Hello World!

결과 :  Hello World!
        Hello World!
```

함수표현식과 함수선언식의 차이는 어떤것이 있을까 ?

```
console.log(helloB()); // Hello World! B
console.log(helloA()); // Error

// 함수표현식
let helloA = function () {
  return "Hello World! A";
};

// 함수선언식
function helloB() {
  return "Hello World! B";
}
```

위 코드를 보면 helloB함수는 정상적인 출력이 이루어지는데 helloA함수는 Error가 난다.
helloB함수는 아래쪽에 선언이 되어있는데 위쪽 console.log(helloB())에서 정상적인 출력이 된다. 이것이 호이스팅이라고 하는 현상이다.

호이스팅은 그냥 간단하게 함수선언식으로 만들어진 함수들은 프로그램시작 전에 코드의 최상단으로 끌어올려진다 라고 생각하면 된다.

그런데 helloA함수는 왜 Error가 날까 ?
함수표현식은 변수에 할당이 될때 함수가 생성되어 프로그램이 읽을 수가 있다. 그래서 호이스팅이 이루어 지지않는다.
즉 변수에 할당이 되지 않았기 때문에, 다른 말로 함수가 선언될 때 할당이 되기 때문에 위에서 사용할 수 없는 것이다.
함수표현식은 변수에 할당 된 후에 그제서야 사용할 수 있게 된다.

# 화살표함수

함수표현식을 간략하게 사용할 수 있게 합니다. es6문법입니다.

```
let helloA = function () {
  return "Hello World! A";
};

let helloB = () => "Hello World! B";

let helloC = (x, y) => {
  let result = x + y;
  return result;
};

console.log(helloA());
console.log(helloB());
console.log(helloC(1, 2));
```

위에서 helloA 함수와 helloB 의 함수는 완전히 동일한 일을 하는 함수이다.
helloA함수를 helloB함수 처럼 축약해서 사용이 가능한 것이다. 사용법은 function키워드를 지우고 () 뒤에 => 를 추가해주면 된다.
return 값이 하나밖에 없는 경우 helloB 처럼 return도 생략이 가능하며 helloC함수 처럼 매개변수를 ()소괄호 안에 넣고 {} 중괄호로 묶어어서 return도 가능하다.
**_ 물론 화살표 함수도 호이스팅의 대상이 아님으로 순서를 잘 지켜서 사용하자 _**
