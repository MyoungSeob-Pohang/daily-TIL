# 객체

객체도 자바스크립트에서 아주 중요한개념으로 자료형문서에서 간략하게 작성을 하였지만 비원시타입중 하나로 한번에 여래 개의 값을 가질 수 있다.
고정되지 않은 동적공간 즉 몇개의 값을 가지고 있어도 된다는 것이며, 객체는 key: value 형태로 되어있다.
일단 객체는 2가지 방법으로 생성할 수 있다.

1. 객체 생성자 방법

```
let person = new Object();
```

2. 객체 리터럴 방식

```
let person = {};
```

객체 리터럴 방식이 생성자 방법보다 만들기 편해서 많이 쓰인다. 객체리터럴방식으로 생성할떄 {} 중괄호 안에 값을 넣으면 된다.
key : value 형식으로 저장해야 하며 이것을 **_프로퍼티(속성)_** 라고 한다. 이 객체에는 어떠한 값(자료형)이 들어와도 저장할 수 있다.

```
let person = {
    key1: "value1", // 문자열
    key2: true, // 참, 거짓
    key3: undefined, // 언디파인트
    key5: [1, 2, 3], // 배열
    key6: function(){ }, // 함수, 객체안에 있는 함수는 메서드라고 한다. 함수가 아닌 다른값들은 멤버라고 한다.
    key7: { // 객체
        key8: "key8",
        key9: "key9,
    }
};
```

이렇게 객체 안에는 문자든 배열이든 함수든 객체든 뭐든지 다 저장시킬 수 있다. 하지만 key는 반드시 문자열로 사용해야 하고 "" 제외 후 사용해야 한다.
그리고 key값이 중복될 순 있지만 먼저 선언된 key이름이 무시 된다. 그래서 권장되지 않는다.

```
let person = {
    key: "value1", // 무시
    key: true, // 출력
};
```

## 객체 값 꺼내오기

```
let person = {
    key1: "value1", // 문자열
    key2: true, // 참, 거짓
    key3: undefined, // 언디파인트
    key5: [1, 2, 3], // 배열
    key6: function(){ }, // 함수
    key7: { // 객체
        key8: "key8",
        key9: "key9,
    }
};
```

만약 위 같은 상황에서 key1의 값만 꺼내고 싶다면 어떻게 해아할까 ? 점표기법/괄호표기법을 사용하면된다.
위 객체에서 key1을 꺼내려면

```
console.log(person.key1); // 점표기법
console.log(person.["key1"]); // 괄호 표기법
```

이렇게 출력하면 된다.
객체이름.프로퍼티이름 or 객체이름.["프로퍼티"] 이다. 괄호표기법은 []대괄호 안에 무조건 "" 문자형으로 넣어주어야 한다. 아니면 변수명으로 인식하기 때문이다.
변수명을 따로 선언해서 사용한다면 "" 제외할 수 있다.

```
const name = "name";
console.log(person[name]);
```

보통은 .점표기법을 많이 사용하지만 괄호표기법은 함수로 객체에 key값을 통해 value를 가지고 오고 싶다라고 하면, 아래와 같이 사용이 가능하다.

```
let person = {
  name: "moonsbi",
  age: 65
};

console.log(getPropertyValue("name"));

function getPropertyValue(key) {
  return person[key];
}
```

## 객체생성 이후 프로퍼티 추가, 삭제, 수정방법

1. 추가방법
   점표기법이나 괄호표기법으로 값을 대입해서 넣으면 추가된다.

```
let person = {
  name: "moonsbi",
  age: 65
};

// 만약 person에 빠진 값이 있다면 ...
person.height = 199;
person["gender"] = "남자";
```

2. 수정방법
   추가방법과 동일하게 값을 대입하여 수정하면 된다.

```
let person = {
  name: "moonsbi",
  age: 65
};

// 만약 person에 빠진 값이 있다면 ...
person.height = 199;
person["gender"] = "남자";

// 만약 person.age값을 변경하고 싶다면 ...
person.age = 99;
person["height"] = 188;
```

**_ 주의사항 _**
위에서 person 객체는 let 으로 선언되어 있다. 하지만 let이 아니라 const로 변경하여도 객체값에 대한 수정, 변경, 추가 등은 이상없이 변경 가능하다.
왜일까 ? person의 객체자체를 바꾸는게 아니기 때문이다. person이 가지고 있는 object를 수정하는 것이지 person 자체를 변경시키는게 아니기 떄문이다.

```
const person = {
  name: "moonsbi",
  age: 65
};

let person1 = {
  name: "moonsbi_A",
  age: 11
};

person.age = 32; // 가능

person = {
  name: "coco",
  age: 30
}; // 객체 자체를 수정하는 행위임으로 const 라서 불가능

person1 = {
  name: "coco",
  age: 30
}; // let 임으로 수정가능
```

3. 삭제방법
   delete 키워드를 사용하여서 삭제가 가능하다.

```
let person = {
  name: "moonsbi",
  age: 65
};

// age를 삭제하고 싶다면 ...
delete person.age;
delete person["age"]
```

하지만 이렇게 지우는 방법은 객체안의 값과 연결을 끊을 뿐이고 실제로 메모리상에는 age값이 지워지지 않는다.
출력시에 age가 나오지는 않지만 실제 메모리상에는 age값이 존재하는 것이다. 그래서 delete를 사용하는 것 보다는 null을 사용한다.

```
let person = {
  name: "moonsbi",
  age: 65
};

person.age = null;
```

이렇게 되면 실제로 age값을 지우는 효과와 같은 효과를 낼 수 있고, 실제 메모리 상에서도 날릴 수 있다.

4. 객체의 함수, 객체 호출 방법
   아까 위에서 객체는 함수 또는 객체를 가지고 있을 수 있다고 하였다. 그 함수와 객체를 호출하는 방법은 아래와 같다.

```
const person = {
  name: "moonsbi",
  age: 65,
  say: function () {
    return "Hello";
  },
  friend: {
    friend1: "coco",
    friend2: "minsu"
  }
};

// 객체 호출
console.log(person.friend.friend1);
console.log(person.friend["friend2"]);
// 함수 호출
console.log(person.say());
console.log(person["say"]());
```

## 함수안에서 맴버사용

```
const person = {
  name: "moonsbi",
  age: 65,
  say: function () {
    return `Hello ${this.name}`; // 혹은 ${this["name"]}
  }
};

console.log(person.say());

결과 : Hello moonsbi
```

여기서 보면 return 문에 백틱(``)이 사용되었고 this.name이라는 키워드를 사용하고 있다. 이 **_ this는 지금 person 즉 자기자신을 가르킨다고 볼 수있다. _**

### 객체 안에 값이 있는지 확인하기

```
const person = {
  name: "moonsbi",
  age: 65,
  say: function () {
    return `Hello ${this.name}`;
  }
};

console.log(person.weigth); // weight 값은 없음으로 undefined 출력
console.log("name" in person); // name이 person에 있니 ? 라는 뜻으로 in을 사용하여 확인한다. 반환되는 값은 true or false 값이다. 그래서 출력은 true로 출력된다.
console.log("height" in person); // false
```
