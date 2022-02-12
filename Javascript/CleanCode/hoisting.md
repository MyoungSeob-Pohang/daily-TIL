# 호이스팅 주의하기

호이스팅은 간단하게 얘기하면 선언과 할당이 분리된 것을 뜻한다. 런타임시 분리가 되는데 런타임시는 동작할 때를 의미한다.
코드를 작성할 때는 이 스코느는 이렇게 동작할 것이다 라고 예상하는데, 런터암이에서 동작할 때는 스코프가 예상대로 움직여 주지 않는다.
그 현상중에 하나가 바로 호이스팅이다. 호이스팅은 var로 선언한 변수가 초기화가 제대로 되어 있지 않았을 때 undefined로 최상단에 끌어올려줄 수 있는 것이다.
즉 선언부만 최상단으로 옮기는 것을 의미하는데 let & const를 쓰면 이런 현상을 잘 겪지 않겠지만, var를 쓰면 이러한 상황이 많다.

```
var global = 0;

function outer() {
  console.log(global);
  var global = 5;

  function inner() {
    var global = 10;

    console.log(global);
  }

  inner();

  global = 1;

  console.log(global);
}

outer();
```

아래 코드의 출력은 어떻게 될까 ? undefined, 10, 1이 나오게 된다. 이전 작성된 문서들을 봤다면 var는 함수 스코프를 가지기 때문에 10과 1이 어떻게 나왔는지는 알 수 있을 것이다. 하지만 undefined는 어디서 왔을까 ? 이게 바로 선언과 할당이 분리된 상황 즉 호이스팅이 동작한 사례이다. 위 코드의 경우

```
function outer() {
  console.log(global);
  var global = 5;
```

이 부분은 사실

```
function outer() {
  var global;
  console.log(global);
  var global = 5;
```

이 코드와 같은 상황이다. 선언과 할당 부분이 메모리 공간을 선언하기 전에 이러한 일이 발생하는 것이다. 이렇게 var키워드를 사용하면 위험한 결과를 초래할 수 있다.

```
function duplicatedVar() {
  var a;
  console.log(a);
  var a = 100;
  console.log(a);
}

duplicatedVar();
```

위 코드도 당연히 첫번째 console이 undefined가 출력될 것이다. 그리고 자바스크립트는 함수도 호이스팅이 된다.  
그래서 보통 함수를 만들 때 const를 사용해서 함수표현식으로 만든 후 바로 할당하는 방식을 사용하는 것이 좋다.

```
const sum = function(){
    return 1 + 2;
}

console.log(sum())
```

이렇게 함수표현식으로 만들게 되면 const sum이 할당이 될 때 함수가 생성이 됨으로 호이스팅이 일어나지 않아 실수를 줄일 수 있고 헷갈리지 않게 개발할 수 있다.

결론은,

1. 호이스팅은 런타임시에 바로 선언을 최상단으로 끌어올려 진다.
2. 코드를 작성할 때 예측하지 못한 실행 결과가 나올 수 있다.
3. 이러한 상황을 탈피하기 위해 var를 사용하지 않는다.
4. 그리고 함수도 호이스팅 되기 때문에 함수표현식을 사용한다.
