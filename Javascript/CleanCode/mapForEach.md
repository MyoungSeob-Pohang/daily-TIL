# map vs forEach

자바스크립트의 배열메서드를 많이 사용하고 나면서 부터 많이 생기는 실수중에 하나가 있다. map과 forEach의 차이를 모르거나 둘중 하나만 사용하는 경우이다.

```
const prices = ['1000', '2000', '3000'];

const newPricesForEach = prices.forEach(function(price) {
    return price + '원';
});

const newPricesMap = prices.map(function(price) {
    return price + '원';
});
```

forEach와 map의 코드적인 관점에서 차이를 보자면 크게 차이가 없다. 동작도 크게 차이가 없을 껀데, 하나하나 값에 접근을 하면서 그에 대한 값을 리턴하고있다. 그럼 무슨차이가 있을까 ? 두 값을 찍어서 출력해보면 그 차이를 확실히 알 수 있다.  
forEach의 출력은 undefined, map의 출력은 1000원, 2000원, 3000원 이다. forEach는 왜 값이 undefined일까 ? mdn문서를 찾아보면 그 결과를 알 수 있지만, 가장 큰 차이는 forEach는 반환값이 없다는 것이다. 그래서 forEach와 map의 차이점 중에 가장 큰 차이가 바로 결과값이다.  
map은 원본 배열을 참조하지만 원본배열을 손상시키지 않고 새로운 배열을 만들어내고, forEach는 주어진 함수를 배열 요소 각각에 실행시키는 것이다. 그래서 forEach는 결과값이 undefined인 것이다. 그럼 많은 사람들이 생각할 것이다, map 쓰면 되고 forEach는 안써도 되겟네 ? 아니다.  
forEach는 값에 대해 함수를 가져와서 쓰거나 출력만 하거나(console.log) 이럴때는 forEach를 쓰는게 맞다. 언어의 명세가 forEach는 함수실행, map은 새로운 배열 반환이기 때문이다.
