# undefined & null

undefined & null 둘다 값으로 쓰이기에는 없다라는 것을 의미한다. undefined 와 null의 차이가 있다는 것이 인터넷에 웃긴 밈처럼 돌아다니는 것이 있는데, 화장실 휴지밈이 생각난다.

![undefined & null](https://github.com/MyoungSeob-Pohang/daily-TIL/blob/main/Javascript/image/null.png)

이러한 짤이 생길 만큼 자바스크립트는 많은 헷갈림을 준다.  
undefined는 만들어 놓고 정의하지 않은 것이고, null은 명시적으로 비어있는 값을 나타내는 값이다. null에 대한 값은 정말 헷갈린다, 아래 코드를 보자.

```
console.log(!null);
console.log(!!null);
console.log(null === false);
console.log(!null === true);

출력
true
false
false
true
```

보기만 해도 헷갈리지 않는가 ? 진짜 너무 헷갈리는데 여기서 끝이 아니다, 수학적인 부분에서 봤을 때 null은 0처럼 동작한다.

```
console.log(null + 12);
console.log(!null + 22);

출력
12
23
```

null은 0, not null은 1값을 가지고 수학적인 계산도 가능해져 버린다. 그럼 undefined는 뭘까?  
undefined는 아무것도 지정하지 않았을 때 무언가의 기본값이라고 할 수 있다. 결국 변수나 무엇을 선언하였지만 값을 할당 하지 않은 상태를 의미한다.

```
let value;

console.log(value);
출력
undefined
```

그럼 undefined로 산술연산을 해보면 어떨까 ?

```
console.log(undefined + 10);

출력
NaN
```

NaN이 나오게 된다. 숫자가 아닌 것이다. null은 0으로 표현되지만 undefined는 Not a Number로 표현이 되는 것이다. 근데 여기서 더 헷갈리게 하는게 있다.

```
console.log(!undefined);

출력
true
```

아니 undefined를 not연산자를 썻더니 true가 되었다. 이렇게 혼란한점이 너무나 많다. 그런데 자바스크립트는 null, undefined의 쓰임새가 굉장히 많다.  
그래서 null, undefined을 활용한 코드를 많이 작성하기 보다는 팀이나 본인이 비어있는 값은 null로 정의하자 undefined로 정의하자 같은 규칙을 정하는게 좋다.

요약하자면

1. null, undefined은 값이 없거나 정의되지 않은경우나 값이없는 것을 명시적인 표현을 하는 것이다
2. null은 숫자로 표현하면 0, undefined는 NaN
3. type으로 보면 undefined는 undefined, null은 Object이다.
4. undefined와 null 모두 쓰임에 조심해야 한다.
