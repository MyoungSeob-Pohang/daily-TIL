# 부정조건문 지양하기

부정조건문은 Not연산이라고 생각하면된다

```
const isCondition = true;
const isNotCondition = false;

if(!isCondition) {
    console.log('거짓인 경우에만 실행);
}

if(isNotCondition) {
    console.log('거짓인 경우에만 실행);
}
```

isCondition은 참 isNotCondition은 거짓이라는 값을 가지고 있는데, 그 값을 Not연산자를 사용하여 뒤집을 수 있다. 위 if문을 보면 둘다 거짓인 경우에만 실행되는게 동일하다. 이래서 부정조건문이 별로인것이다. 그리고 또 하나 문제가 있는데, if-else문을 살펴보면 if는 거의 참인 경우 실행될 코드가 들어가고 else는 거의 거짓을 경우 실행될 코드들이 들어간다. 굳이 조건을 뒤집어서 if문일때 not을 써서 거짓일 경우 실행될 코드로 짜야지, 할 필요가 없다는 것이다. 너무 생각을 꼬아서 해야하지 않을까 ?  
결론은 아래와 같다.

1. 부정조건문은 생각을 여러번 해야 할수 있다.
2. 프로그래밍 언어 자체로 if문이 처음부터 오고 true부터 실행 시킨다.

그럼 부정조건문은 언제 사용하는 것이 좋을까 ? EarlyReturn사용할 때나 유효성 검증을 할 때, 보안이나, 계속 검사 해야 하는 로직이 있을 경우 사용하면 좋다.
항상 잊지말자, 사람이 읽기 좋고 명시적인 코드가 좋은 코드이다.
