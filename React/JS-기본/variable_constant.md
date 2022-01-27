# 변수

수학에서와 같이 변하는 수, 변할 수 있는 수이며 하나의 값을 저장할 수 있는 저장공간이다.
var , let과 같은 키워드로 선언할 수 있으며 선언 후 값을 할당하지 않고 나중에 할당하더라도 괜찮다.

## 변수 선언 방법

```
let age = 30;
var name = 'MoonsBi';
```

이와 같이 선언하며 age라는 이름에 30이란 수를 저장하고, name이라는 이름에 문자열 MoonsBi를 저장하게 된다.

```
console.log(age) => // 30 출력
console.log(name) => // MoonsBi 출력
```

## 변수 규칙

1. \_, $를 제외한 기호를 사용할 수 없다.
2. 숫자로 시작할 수 없다
3. 예약어로 사용할 수 없다.

```
let +age = 10; // Error + 기호사용
let 1value = 20; // Error 숫자로 시작
let $name_ = 10; // OK
let class = 'test' // Error class라는 예약어 사용

```

## var와 let의 차이

실제 var와 let은 키워드만 다르지 선언방법, 사용방법은 동일하다.
하지만 var는 함정이 존재한다.

```
var age = 10;
console.log(age) // 10 출력
var age = 'Moonsbi';
console.log(age) // Moonsbi 출력

```

이와 같이 같은 변수명을 가진 age가 여러번 선언되어서 값이 변경될 수 있다.
코드가 위와 같이 짧다면 작성자가 한번에 알아보고 고칠 수 있지만 10을 가진 age와 Moonsbi를 가진 age 사이에 수많은 코드가 존재한다면,
작성자가 원하지 않는 값을 출력할 수도 있다.
이러한 함정에 빠지지 않기 위해서 let이라는 키워드로 사용한다.  
**_ let은 변수를 중복해서 사용하는 것을 허용하지 않는다. _**

```
let age = 10;
console.log(age) // 10 출력
let age = 'Moonsbi'; // Error
console.log(age)

```

# 상수

수학에서와 같이 변하지 않는 수 이며, 하나의 값을 저장할 수 있고 변수와 달리 한번 선언하면 변하지 않고 변경도 불가능 하다.

## 상수 선언 방법

```
const MAX_VALUE = 100;
const PI = 3.14;

```

const 라는 키워드를 사용하며 뒤에오는 상수명은 대문자로 모두 써주는게 관례이다. 연결되는 단어는 \_ 기호를 사용하여 연결한다.
상수는 변하지 않는 값이니 재할당이 불가능 하다.

```
const PI = 3.14;
PI = 3; // Error
const PI = 1; // Error

```

그리고 값을 초기에 넣어주지 않는 것도 에러가 발생한다. 나중에 값을 넣을 수 없기 때문이고 선언 후에는 반드시 값을 정해주어야 한다.

```

const PI; // Error

```
