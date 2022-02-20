# argument & parameter

자바스크립트나 다른 언어들을 사용할 때에 함수에서 넘기는 인자, 혹은 매개변수를 뭐라고 부를까? 검색해보면 수 많은 사람들이 궁금해 하는 것인데 도대체 argument와 parameter는 무엇이 다른 걸까?  
mdn문서에 보면 parameter는 함수가 선언된 부분은 parameter라고 정의하였고, 즉 함수 부분에 정의된 것은 다 parameter이고 함수를 사용하는 곳은 다 argument인 것이다.

```
function example(parameter) {
    console.log(parameter);
}

const argument = 'foo';
example(argument);
```

위와 아래 코드를 보면 더 이해하기 쉬울 것이다.

```
매개변수 ( parameter )

function axios(url) {
    // ...
}

실제로 사용되는 인자 or 인수 ( argument )

axios('http://www.naver.com');
```
