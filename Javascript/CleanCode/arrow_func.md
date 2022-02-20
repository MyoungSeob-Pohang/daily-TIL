# 화살표 함수

예전과 다르게 이제 자바스크립트에서 화살표 함수는 안쓸래야 안쓸 수가 없다. 그만큼 편하고 간결하게 읽힌다는 이유로 화살표 함수를 많이 사용한다. 하지만 이 부분도 정확하게 알고 사용해야한다. 아래 코드를 보자.

```
const user = {
    name 'koko',
    getName: () => {
        return this.name;
    }
}

user.getName(); // undefined
```

왜 undefined일까 ? 화살표함수는 scope를 Lexical scope를 가지게 된다. 이 Lexical scope는 함수를 어디서 호출하는지가 아니라 어디에 선언하였는지에 따라 결정된다. 그렇기 때문에 getName메서드의 this는 user객체가 아니라 그 위에 상위를 바라보고 있다는 것이다. 그렇기 때문에 아래와 같이 사용해야한다.

```
const user = {
    name 'koko',
    getName() {
        return this.name;
    }
}

user.getName(); // koko
```

이렇게 메서드에서 화살표함수를 사용할 때에는 주의하면서 사용해야 한다. 이 뿐만 아니다. 화살표 함수에서는 arguments객체도 사용할 수 없고, call, apply, bind 등등 사용할 수 없는 것들이 많다. arguments를 사용하고 싶다면 rest Parameter를 사용해야 한다. 이렇게 화살표함수는 일반적인 자바스크립트의 함수가 가지고 있는 모든 것들을 계승해서 가지고 있지는 않다. 그렇기 때문에 맹목적으로 화살표함수를 사용하지말고 주의해서 사용해야 하는 것이다.  
아 그리고 화살표 함수로 만든 함수는 생성자로도 사용할 수 없다. 그렇기 때문에 new 키워드와 사용할 때에도 주의해야 한다.
