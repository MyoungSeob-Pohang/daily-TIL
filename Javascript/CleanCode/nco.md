# Nullish coalescing operator

최근에 나온 연산자로 해석하자면 null 병합 연산자이다.

```
function createElement(type, height, width) {
    const element = document.createElement(type || 'div');
    element.style.height = String(height || 10) + 'px';
    element.style.width = String(width || 10) + 'px';
}
```

위 와같은 코드를 보면 or연산자를 잘 활용하여 디폴트 값도 잘 정해주고 있다. || 기준으로 좌항이 true인지 false인지 판별하여 false면 10이 할당이 되는 것이다.

하지만 내가 width, height 값을 0으로 주고 싶다면 ? 실제로 createElement('div', 0, 0) 이렇게 호출을 하게 되면 width와 height의 값은 모두 10으로 정해진다. 0은 바로 false값을 가르키기 때문이다. 결론은 OR연산자로 비교하면 0이 들어가면 좌항은 무조건 false인 상태이다. 이럴때 사용할 수 있는게 null 병합 연산자이다.  
키워드는 ?? 물음표 2개이다.

```
function createElement(type, height, width) {
    const element = document.createElement(type ?? 'div');
    element.style.height = String(height ?? 10) + 'px';
    element.style.width = String(width ?? 10) + 'px';
}
```

이 null 병합 연산자는 좌항의 값이 null이거나 undefined 일 때만 작동을 한다. 이렇게 하면 width, height 값이 모두 0px로 잘 들어간 것을 볼 수 있다.
실제로 실무에서도 굉장히 많이 쓰이지만 부작용이 하나가 있다. ||연산자와 ?? 연산자를 상황에 맞게 잘 써야하는데 편하다 보니 모두 ?? 연산자를 사용하여 개발을 해버리기 때문이다. 꼭 유념해야한다. null이거나 undefined 일 때만 ??를 사용하자. 그리고 아래와 같은 오류도 있다.

```
No chaining with AND or OR operators


null || undefined ?? "foo"; // raises a SyntaxError
true || undefined ?? "foo"; // raises a SyntaxError
```

이렇게 여러개의 평가를 넣으면 에러가 난다. 자바스크립트 문법자체에서 실수가 너무 많아 Logical Expressions and coalesce.. 혼합해서 사용하지 말라는 에러이다. 이런 경우에는 ()를 감싸서 연산자 우선순위를 정해주면된다. 실제 ?? 의 연산자 우선순위는 7번째로 낮은편에 속한다.

```
(null || undefined) ?? "foo"; // returns "foo"
```
