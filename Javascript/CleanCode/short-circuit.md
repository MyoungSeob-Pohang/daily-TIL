# 단축평가

[단락 회로 평가](https://github.com/MyoungSeob-Pohang/daily-TIL/blob/main/Javascript/JS-%EC%9D%91%EC%9A%A9/short_circuit_rating.md) 에서도 한번 얘기했던 내용이다.

조금 더 쉽게 코드로 얘기해보려고 한다. AND와 OR연산자 중에 AND 연산자 부터 보자.

```
true && true && '도달O'
```

위 코드는 AND연산자를 사용하였다. 자 지금 보면 AND연산자는 모두 true여야 true를 반환한다. 그럼 첫번째 값은 true 확인, 두번째 값도 true 확인, 그럼 3번째 값인 도달도 당연히 검사해서 true이면 true를 최종적으로 반환할 것이다.

```
true && false && '도달X'
```

이 코드는 어떨까 첫번째 값은 true이니 넘어가고 다음 값은 false를 만났다. 자 그러면 AND 입장에서 더 검사할 필요가 있을까 ? 3번째 값이 true이던 말던 AND연산자는 무조건 false다. 그럼 당연히 false를 만나자마자 끝나버리니 뒤에 도달까지 검사를 하지 않는 것이다.  
OR연산자는 어떨까 ?

```
false || false || '도달O'
```

자 OR 연산자는 하나만 true라도 true를 반환한다. 첫번쨰 값 false이니 두번쨰 값을 본다. 두번쨰 값도 false 이니까 세번째 값을 본다. 그럼 결국 도달이 보일 것이다.

```
true || false || '도달X'
```

이 코드는 어떨까? 첫번째 값이 true다. 그럼 2번째 3번째 n번째 값을 알 필요가 있을까 ? 당연히 없다. 그럼 true를 만나자마자 거기서 종료되면서 true를 리턴하는 것이다.

자 그럼 이 현상을 어떻게 사용할 수 있을까, 아래 코드를 보자.

```
function fetchData() {
    if (state.data) {
        return state.data;
    } else {
        return 'Fetching';
    }
}
```

위 코드를 보면, 바로 아 코드가 너무 길다, 삼항연산자로 줄일 수 있겠는데 ? 라고 생각이 든다. 그래서 줄여보면 .

```
function fetchData() {
    return state.data ? state.data : 'Fetching';
}
```

한줄로 깔끔하게 떨어진다. 하지만 이것보다 더 좋은 방법이 바로 OR연산자를 사용하는 방법이다. OR연산자를 쓰기 가장 좋은 예는 이런 기본값을 표현할떄 가장 편리한데,

```
function fetchData() {
    return state.data || 'Fetching';
}
```

이렇게 사용이 가능하다는 말이다. state.data가 있으면(true) 그대로 반환하고 없으면 'Fetching'은 truthy한 값이니 바로 반환한다. 삼항연산자 보다 훨씬 더 깔끔하고 알아보기 쉽지않나 ? 지금 당장은 쉽게 안될 수 있지만 항상 의식하고 사용하다보면 언젠가는 쉽게 사용할 날이 올것이다!
