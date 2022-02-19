# hasOwnProperty

hasOwnProperty의미 그대로 프로퍼티를 가졌느냐 라는 뜻이다.

```
const person = {
    name: 'moons',
}

person.hasOwnProperty('name'); // true
person.hasOwnProperty('age'); // false
```

위와 같이 객체가 값을 가지고 있느냐 라고 물어보는 것이다. 하지만 mdn문서에서도 나오는데, 객체의 프로퍼티를 순회할때 hasOwnProperty를 사용하지만 hasOwnProperty명칭을 자바스크립트에서 보호하지 않는다. 보통 자바스크립트 객체는 Object.prototype에서 확장해서 객체가 발전되는데 hasOwnProperty는 사용을하면은 다른것과 겹칠 수 있다는 것이다. 이것은 큰 문제인데 hasOwnProperty를 사용했을 때 다른 키워드에있는 hasOwnProperty를 호출할 수 있다는 것이다. 그래서 사용할 때에는 Object.prototype.hasOwnProperty.call 이렇게 사용을 하여야 안전하게 사용할 수 있다는 것이다. 말이 어려운데 코드로 보자.

```
const foo = {
    hasOwnProperty: function () {
        return false;
    },
    bar: 'string',
};
```

foo라는 객체는 hasOwnProperty라는 문자열을 반환하는 메서드를 가지고 있고 bar라는 문자열을 가진 값도 가지고 있다. foo에 hasOwnProperty라는 키워드를 사용해서 bar가 있는지 확인한다고 했을 때 아래와 같이 코드를 작성할 것이다.

```
foo.hasOwnProperty('bar'); // hasOwnProperty
```

위의 코드의 출력값을 보면 true, false이런 값이 아닌 hasOwnProperty가 출력이 된다. 이렇게 때문에 Object에서 가지고 와야한다는 것이다.

```
Object.prototype.hasOwnProperty.call(foo, 'bar'); // true
```

동작결과가 위 코드와 완전히 다르다. 어떻게 보면 자바스크립트 이슈라고 볼 수도 있는데, let, const, function이라는 키워드는 무언가에 보호를 받지만 hasOwnProperty는 보호받지 못하기 때문에 prototype에 접근해서 좋은건 없지만 사용하려면 prototype의 call형태로 접근해야한다. 이게 귀찮다면 함수로 정의해서 사용하면 된다.

```
const person = {
    name: 'moons',
}

function hasOwnProp(targetObj, targetProp) {
    return Object.prototype.hasOwnProperty.call(targetObj, targetProp)
};

Person.hasOwnProp(person, 'name'); // true
```
