# Min - Max 경계 다루기

```
function genRandomNumber(min, max){
    return Math.floor(Math.random() * (max - min + 1)) + min;
}
```

위 코드는 최소값과 최대값을 받아서 난수를 생성하는 함수이다.
min / max의 판별에 가장 좋은 방법은 값을 미리 상수로 선언해두는 것이다.

```
const MIN_NUMBER = 1;
const MAX_NUMBER = 45;

genRandomNumber(MIN_NUMBER, MAX_NUMBER);
```

위 코드는 상수 MIN_NUMBER와 MAX_NUMBER를 선언 후 genRandomNumber에 값을 넘겨주고있다. 지금 보면 누가봐도 함수 호출부만 보면 아 이 함수는 최소값과 최대값을 받아서 랜덤한 숫자를 만들어내는 함수구나 라는 것을 알 수 있다. 그래서 함수의 구현부를 보지 않아도 어떠한 역할을 하는지 알수있기 때문에 이런식으로 네이밍을 하는 것이 좋다.  
그런데 이럴떄 min , max 값은 문제가 생길수 있는데 바로 어느정도 포함할 것이냐에 대한 것이다.  
팀마다 다르겠지만, 이상, 이하, 초과, 미만 어떤 것으로 할지 경계가 애매하게 느껴질 수 있다. 어떤 사람은 습관적으로 포함하는 경우도 있을 것이고 어떤 사람은 당연히 포함안하는 경우도 있을 것이다.

```
const MAX_AGE = 20;

function isAdult(age){
    if(age >= 20){
        ...
    }
}
```

위 코드를 보자, if문 안에 조건에 따라서 성인이 될 수도, 안 될수도 있다. 즉 최소값, 최대값이 포함되는지 안되는지 애매하다는 것이다. 이럴때는 헷갈리지 않도록 선언된 상수값에 표시를 해주는 것이 좋다.

```
const MIN_NUMBER_LIMIT = 1;
const MAX_NUMBER_LIMIT = 45;
```

이와 같이 LIMIT을 붙여서 초과, 또는 미만으로 설정 할 수도 있고 만약 이상 이하라면,

```
const MIN_IN_NUMBER = 1;
const MAX_IN_NUMBER = 45;
```

IN을 넣어서 포함이 된다는 것을 명시적으로 포함시켜주는 것이 좋다.

정리하자면 .

1. 최소값과 최대값을 다룬다
2. 최소값과 최대값 포함 여부를 결정해야 한다 (이상, 초과 / 이하, 미만)
3. 혹은 네이밍에 최소값과 최대값 여부를 표현한다.
