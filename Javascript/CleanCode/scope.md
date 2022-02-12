# 함수 스코프 / 블럭 스코포

scope : 범위라는 뜻으로 var, let, const는 각각 영역이 존재한다고 생각하면 된다.
보통 전역변수를 사용하지 말라는 이야기를 많이 하는데 아래 코드로 예제를 보자.

```
var global = "전역";

if (global === "전역") {
  var global = "지역";
  console.log(global);
}
console.log(global);
```

위 출력은 둘다 지역으로 출력이 된다. 기대했던 바는 if문 안에서 출력했을 때 지역으로 출력이되고 if문 밖에서는 전역으로 출력이 되는것을 원했는데, var로 선언하게 되면 if문 안에서 전역공간 까지 영향을 주게 되었다. 왜그럴까 ? var는 함수단위 스코프라서 그렇다. if는 함수가 아니기 때문에 전역공간에 침범이 된 것이기 때문이다.
그래서 블럭단위로 바꾸지 않는 이상 계속 불안요소를 가지고 있는 것이다. 그럼 let은 어떨까 ?

```
let global = "전역";

if (global === "전역") {
  let global = "지역";
  console.log(global);
}
console.log(global);
```

출력은 지역, 전역 순으로 출력된다. let은 var와 다르게 if문 안에서만 지역으로 사용되고 밖에서는 전역으로 잘 보여주게 된다. 블럭이란 것은 쉽게 얘기하면 {} 중괄호 영역이다. let은 블럭 스코프라서 {}안에서의 값이 전역에 영향을 주지 않는다. 꼭 if문 뿐만아니라 그냥 {}안에만 묶어도 동일한 결과를 가져온다.

```
let global = "전역";

{
  let global = "지역";
  console.log(global);
}
console.log(global);
```

이렇게 해서 var 보다 let & const가 안전하다고 하는 것이다. 하지만 let보다도 const가 훨씬 더 좋다. let을 쓰면 재할당도 가능하고 값이 변경될 수 있으니 더 편하지 않을까 라는 생각을 하지만 const도 재할당?이 가능하다. let과 const모두 {} 블럭 스코프이다. 그리고 재할당이라는 키워를 생각하고 아래 코드를 보자.

```
const person = {
  name: 'jang',
  age: 30,
}

person = {
  name: 'jang',
  age: 30,
}
```

이렇게 되면 아래 선언된 person은 재할당이 안되기 때문에 당연히 오류가 난다. 위 person에서 선언과 동시에 할당을하고 있고 아래에서 재할당을 하는데 const는 재할당이 불가능하기 때문에 값변경이 불가능하다. 그럼 person의 값은 변경이 불가능할까 ? 가능하다.

```
const person = {
  name: 'jang',
  age: 30,
}

person.name = 'lee';
person.age = 22
```

지금보면 person을 재할당하는게 아니라 person내부의 객체값만 바꾼 것이다. 이러면 오류가 발생하지 않는다. 왜냐 ? person이라는 const 자체를 변화시킨게 아니기 때문이다.
객체가 아니라 배열도 마찬가지다.

```
const person = [
  {
    name: "jang",
    age: 30
  }
];

person.push({
  name: "lee",
  age: 22
});
```

person이라는 배열안에 객체하나를 선언해 두고 다른 객체를 push로 배열에 추가하였다.  
자 그럼 결론적으로 봤을 때 const는 재할당만 금지된다, 때문에 본연의 객체 그리고 배열 같은 레퍼런스 객체들을 조작할 때 전혀 이상이 없는 것이다.
