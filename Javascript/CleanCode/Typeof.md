# 타입검사

## typeof

프로그래밍 언어를 다루면서 타입검사는 누구든 한번씩은 해봤을 것이다. 그리고 JS에서는 typeof 연산자를 사용해서 타입검사를한다.  
근데 이 typeof연산자는 아주 재미있는 연산자이다. typeof는 결국 문자열로 반환하는게 특징인데 아래 코드를 보면 typeof 연산자 뒤에 나오는 것을 검사 하여서 문자열로 반환이 된다.

```
console.log(typeof "문자열");
console.log(typeof true);
console.log(typeof undefined);
console.log(typeof 123);
console.log(typeof Symbol());

출력 :  string
        boolean
        undefined
        number
        symbol
```

하지만 이 typeof검사를 할때 typeof로 모든게 커버가 되는것이라고 생각하는 사람이 있는데 그건 정답이 아니다.
자바스크립트 타입을 크게 분류해보자면 PRIMITIVE(원시값) VS REFERENCE(자료형) 두 개로 나눌 수가 있는데, REFERENCE는 typeof로 감별해내기 굉장히 어렵다.

```
function myFunc() {}
class MyClass {}

console.log(typeof myFunc);
console.log(typeof MyClass);
```

이 코드의 결과는 둘다 function으로 출력이 된다. 또한 아래 같은 코드도 있다.

```
const str = new String("문자열");

console.log(typeof str);
```

String형으로 생성했는데 결과값은 string가 아니라 object가 나온다. 이 처럼 REFERENCE 값은 감별해내기가 어렵고 그만큼 typeof가 만능은 아니라는 것이다.  
가장 큰 문제는 바로 null 이다. typeof null 을 보면 결과가 object로 나온다. 이게 가장많은 문제와 헷갈림을 가지고 있다. 자바스크립트에서도 인정을 한 것인데, 이 부분은 결국 언어적인 오류가 맞다. 자바스크립트에서 언어가 계속 발전해오면서 수정이 불가능하다고 판단해서 수정이 되지 않고 있다. 이러한 치명적인 오류도 가지고 있고, 또한 **자바스크립트는 동적으로 변하는 언어기 때문에 타입도 동적으로 변한다.** 그래서 타입을 검사할 때는 많은 주의를 기울여야 한다.

## instanceof

또하나 많이 쓰이는 것이 있는데, instanceof 연산자이다. typeof와 비슷하다.
객체의 프로토타입체인을 검사하는 것인데, 예제 코드로 알아보자.

```
function Person(name, age) {
  this.name = name;
  this.age = age;
}

const moonsbi = new Person("moonsbi", 50);

console.log(moonsbi instanceof Person);
```

moonsbi는 new연산자를 사용해서 Person이 되었는데, 이것을 instanceof로 검사해보니 결과가 True가 나온다. 결국 moonsbi가 Person이냐 ? 라고 물어보는 것과 같은 것이다.

```
function Person(name, age) {
  this.name = name;
  this.age = age;
}

const moonsbi = new Person("moonsbi", 50);

const koko = {
  name: "koko",
  age: 99
};
console.log(moonsbi instanceof Person);
console.log(koko instanceof Person);
```

이렇게 검사하면 koko는 Person이 아니기 때문에 false가 출력이 된다.

```
const arr = []
const func = function() {}
const date = new Date()

arr instanceof Array
func instanceof Function
date instanceof Date
```

위 instanceof의 결과는 True이다. 그런데 여기서 또 웃긴게,

```
const arr = []
const func = function() {}
const date = new Date()

arr instanceof Object
func instanceof Object
date instanceof Object
```

이렇게 하여도 instanceof의 결과가 True이다. 결국 REFERENCE 타입이기 때문에 최상위는 Object라는 것이다. 이렇게 타입검사가 어려움이 많다. 그래서 또 자바스크립트는 또 하나의 타입검사를 제공한다.

## Object.prototype.toString.call()

Object.prototype을 역이용해서 오브젝트에서 값을 가지고 오는 것이다. 아래 코드를 보자.

```
const arr = [];
const func = function () {};
const date = new Date();

console.log(Object.prototype.toString.call(arr));
console.log(Object.prototype.toString.call(func));
console.log(Object.prototype.toString.call(date));

출력

[object Array]
[object Function]
[object Date]
```

이렇게 대괄호 안에 타입이 나오게 되는데 빠짐없이 다 감지하는 것을 볼 수 있다.  
이렇게 자바스크립트는 타입검사를 할때 많은 헷갈림을 주기 때문에 하나하나 검사할 때 조심해서 검사를 해야한다.

요약하자면

1. 자바스크립트는 동적인 타입이라 타입도 동적이라 타입검사가 어렵다
2. 하나하나 잘 찾아서 검사를 해야하고 상황에 맞게 그에 맞는 타입검사를 해야한다.
