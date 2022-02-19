# Object Destructuring

Object Destructuring 구조 분해 할당을 뜻한다. 자바스크립트에서 객체를 구조 분해할 것인데 이미 많이 사용하고 있을 것이다. 특히 본인은 리엑트 들어가면서 많이 썻던 것 같다.

```
function Person(name, age, location) {
    this.name = name;
    this.age = age;
    this.location = location;
}

const moon = New Person('moon', 32, 'korea');
```

위와 같은 코드는 많이 사용했을 것이다. 하지만 위 코드의 문제점은 name, age, location의 순서가 정해져 있다는 것이고 만약에 age를 넣고 싶지않다거나 값이 없을 때에는 undefined값을 넣어주어야 한다. 이럴 때 겍체 구조 분해 할당이 빛을 발하는데 간단하게 사용할 수 있다.

```
function Person({name, age, location}) {
    this.name = name;
    this.age = age ?? 32;
    this.location = location ?? 'korea';
}

const moon = New Person({
    name: 'moon',
});
```

코드를 봤을 땐 크 차이가 없어보이지만 아래 코드는 순서를 지키지 않아도 된다. 그리고 넣고 싶지 않은 값이 있다면 그냥 안넣어도 된다. 넣지 않은 값은 널병합연산자 ??를 통해 기본값을 정해줘야 겠지만 그럼에도 불구하고 누가봐도 명확하게 볼 수 있다고 생각한다.  
그리고 not null과 같은 필수적으로 넣어줘야 하는 코드가 있다면 아래와 같이도 짤 수있다.

```
function Person(name, {age, location}) {
    this.name = name;
    this.age = age;
    this.location = location;
}

const options = {
    age: 32,
    location: 'korea',
}

const moon = New Person('moon', options);
```

여러 라이브러리의 option받는 방식과 동일하게 코드를 작성하였고 이전 같았으면 options를 인자로 넘겨주면 this.age = options.age 이렇게 작성을 해야 했지만 지금은 객체 구조 분해 할당이 생기면서 아주 손쉽게 값을 받을 수 있게 되었다. Person 함수에서 구조분해 하여 age와 location을 받고 있고 호출할때는 name은 필수값으로 꼭 넣어주고 뒤에 options를 인자로 넘겨주고 있는 것이다.  
리액트에서 props를 받을 떄 {}로 받는 것도 객체 구조 분해 할당인 것이다.
