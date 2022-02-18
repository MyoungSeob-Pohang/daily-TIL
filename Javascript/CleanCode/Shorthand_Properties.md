# Shorthand Properties

css의 margin을 보면 아래와 같은 코드를 가끔 볼 수 있다.

```
margin-top : 10px;
margin-right : 5px;
margin-bottom : 10px;
margin-left: 5px;
```

위 코드는 아래와 같이 줄여볼 수 있다.

```
margin: 10px 5px 10px 5px;
```

더 줄일 수도 있다.

```
margin: 10px 5px;
```

background, padding 등 많은 값 들이 축약 될 수 있다. 위와같은 것이 css의 Shorthand Properties 이다. 그럼 js는 어떨까 ?

```
const firstName = 'moonsbi';
const lastName = 'sim';

const person = {
    firstName = 'moonsbi',
    lastName = 'sim',
    getFullName: function () {
        return this.firstName + ' ' + this.lastName;
    }
}
```

위 코드는 정말 흔하게 볼 수 있는 코드이다. Shorthand Properties가 정확하게 뭔지 모르고 사용하고 있는 사람도 있고 어떤 것은 하는데 어떤 것은 안하는 사람도 있다.  
위 코드는 결론적으로 Shorthand Properties가 적용 안된 코드이고 적용된 코드는 아래와 같다.

```
const firstName = 'moonsbi';
const lastName = 'sim';

const person = {
    firstName,
    lastName,
    getFullName() {
        return this.firstName + ' ' + this.lastName;
    }
}
```

코드를 보면 정확하게 알 것이다. 축약이 되는 부분이 많이 있다.  
Concise Method 컨사이즈 메서드라고 간결한 메서드라는 뜻인데 자바스크립트에 메서드를 조금 더 명시적으로 보기 편하게 만들어준다. 지금 위의 getFullName함수가 :과 function 이 사라진 것을 볼 수있다.
하지만 보기 편해졌지만 내부 동작을 모르면 안된다. 사실 getFullName는 함수가 아닌 메서드이고 Shorthand Properties를 사용해 firstName도 축약해서 담은 것들인데 무슨 값인지 모르거나 막 쓰면 안된다.  
키와 벨류가 동일한 형태 혹은 변수에 담은 형태로 표현되었을 떄 간결하게 표현할 수 있는 모든 것들이 문법에서 추가적으로 지원해주는 것이고 ES2015+ 부터 등장한 모던 자바스크립트 형태의 예시 라는 것을 알고 있으면 차후에 큰 실수를 줄일 수 있을 것이다.
