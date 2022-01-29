# 단락회로평가

단락회로평가 라는 것은 논리연산자의 특성에 관한 것으로 쉽게 얘기하면 true false값에 따라서 뒤에 식을 읽냐 안읽냐의 차이이다.

논리연산자는 3가지로 구성이 되는데 AND(&&) OR(||) NOT(!) 이다.
AND는 둘 다 참이면 true 아니면 false
OR는 둘 중 하나라도 참이면 true 아니면 false
NOT은 true면 false, false면 true

```
console.log(true && true);
console.log(false && true);
```

위 코드를 보면 AND 연산으로 첫번째 true && true는 첫번째가 true이고 두번째도 true임으로 둘다 확인을 해야한다.
하지만 false && true는 첫번째가 false니 무조건 false가 반환되어야 한다. 뒤에 true는 읽을 필요가 없는 것이다.
이런 내용을 단락회로 평가라고 한다.

OR 조건은 어떨까 ?

```
console.log(true || true);
console.log(false || true);
```

첫번째 true || true는 첫번째가 true이니 더 안읽고 true를 반환하면 된다. 뒤에 무슨값인지 읽을 필요가 없다는 뜻이다.
하지만 두번째인 false || true 는 첫번째가 false니 뒤에 값을 읽어야 한다.

## truthy, falsy로 단락회로평가 하기

```
const getName = (person) => {
  if (!person) {
    return "객체가 아닙니다.";
  }
  return person.name;
};

let person;
const name = getName(person);

console.log(name);
```

위 코드는 Truthy & Falsy 문서에서 사용한 함수이다. 단락회로 평가를 사용하면 getName함수를 더 짧고 간결하고 멋지게 짤 수 있다.

```
const getName = (person) => {
  return person && person.name;
};

let person;
const name = getName(person);

console.log(name); // undefined
```

person변수는 undefined이다. getName함수에 undefined값이 전달되면서 AND 연산을 하고 있다.
return person 에서 벌써 false이기 때문에 뒤에 person.name은 읽을 필요가 없어지기 때문에 바로 person을 반환한다.
그럼 출력이 바로 undefined가 된다.

```
const getName = (person) => {
  const name = person && person.name;
  return name || "객체가 아닙니다.";
};

let person = {name : 'moonsbi' };
const name = getName(person);

console.log(name); // moonsbi
```

상수 name에 AND 연산 후 결과를 담고 or 연산을 통해 리턴하고 있다. 차례대로 순서를 보면

1. person 변수에 객체가 담긴다.
2. name 상수에 getName 함수를 호출하여 person이 가진 객체를 넘겨준다.
3. getName함수에서 객체를 전달 받아서 보니 person.name 이 moonsbi라는 값을 가지고 있어서 그 값을 name에 저장한다.
4. return 할때 name의 값이 True 이니 OR 조건은 뒤의 식을 읽을 필요가 없어서 moonsbi가 리턴 된다.
5. 출력
