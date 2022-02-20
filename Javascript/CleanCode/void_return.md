# void & return

다른언어를 배웠던 사람이라면 void키워드가 무엇인지 알것이다. 함수의 반환이 없는 것을 의미하고 return은 함수가 무엇인가를 반환한다 라는 것을 의미한다.

```
function handleClick() {
    return setState(false);
};

function showAlert(message) {
    return alert(message);
}
```

위 코드를 보자. 위에 코드는 리액트에서 많이 보던 코드이고 아래에는 그냥 js에서 많이보던 코드이다. 위와같은 경우 이상한점이 하나 보일 것이다. 딱히 문제가 없다고 할 수 있지만 setState나 alert는 void 즉 함수의 반환이 존재하지 않는다. 실행하고 바로 끝이나기 떄문에다. 그렇기 때문에 위 코드에서 모두 return은 필요가 없다. 그럼 무엇을 return하게 되냐면 바로 undefined이다. 자바스크립트는 아무런 return이 없을 때 undefined를 리턴한다. 그렇기 때문에 코드를 작성할 때 위와 같은 코드를 작성하지 않게 끔 노력해야 하고 라이브러리를 사용할 때에도 이 라이브러리가 리턴하는 값이 있는지 없는지 잘 알고 써야한다.  
실제로 예전 html 안에서 inline로 사용할 때 javascript:void(0); 이런식으로 많이 사용해봤던 사람들은 알것이다. 자바스크립트에서도 void를 지원하지만 많은 사람들이 사용하지 않기도 하고 귀찮기도 하다.  
함수명에서 is 혹은 get 이런식으로 시작할 때에는 보통 다 return이 있는 함수들이기 때문에 return을 사용하고 그게 아니라면 리턴을 하지않는다는 그러한 약속을 만들어 두어도 좋다.
