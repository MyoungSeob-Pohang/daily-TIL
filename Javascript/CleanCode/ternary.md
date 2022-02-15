# 삼항연산자

여러분들은 삼항연산자를 얼마나 사용할까 ? 나도 리액트를 사용하기 전에는 정말 잘 안썻던 연산자이다. 하지만 리액트를 사용하면서 굉장히 많이 쓰게 되었다.  
삼항연산자를 사용할 때 케이스에 대한 일관성이 필요하다고 생각한다. 삼항연산자는 3개의 피 연산자를 가지고 ?, : 두개를 사용하여 ?를 기준으로 왼쪽은 조건 참 : 거짓으로 사용할 수 있다. 참, 거짓은 식이 들어가야 하는 것도 잊지말자.

삼항연산자의 예시를 하나 보자

```
function example() {
    return condition1 ? value1
    : condition2 ? value2
    : condition3 ? value3
    : value4;
}

function example() {
    if(condition1) { return value1; }
    else if(condition2) { return value2; }
    else if(condition3) { return value3; }
    else { return value4; }
}
```

위 코드는 인터넷에 떠도는 코드중에 하나인데 삼항연산자를 보면 굉장히 가독성이 떨어진다. 하지만 아래 if else문을 보면 사실 위 삼항연산자 코드와 동작, 로직이 모두 동일하다. 과연 어떤게 더 보기 좋고 어떤게 사용하기 좋을까 ? Poco Jang님은 차라리 이런상황일 때는 switch case문을 사용하라고 한다. 사실 나는 삼항연산자를 1개이상 절대 연결해서 사용하지 않고 if-else를 사용하였다. 하지만 실제로 생각해보니 switch문을 쓰는게 훨씬 더 가독성이 좋을 거 같다.

```
const example = condition1
    ? a === 0 ? 'zero' : 'positive'
    : 'negative
```

위 코드도 마찬가지다, 사실 나는 이렇게 쓸일이 없지만 실제로 이렇게 사용하게 된다면 a === 0 ? 'zero' : 'positive' 부분을 괄호로 묶어서 사용하는게 좋다.

```
const example = condition1
    ? ( a === 0 ? 'zero' : 'positive' )
    : 'negative
```

개발자마다 사용하는 기준이 따로 있겠지만 취향차이라고 하지만 실제 사람이 봤을 때 알아보기 쉽게 작성하도록 항상 고민해야 한다.
