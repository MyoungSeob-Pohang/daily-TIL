# Rest Parameters

Rest Parameters는 의역하자면 나머지 매개변수를 뜻한다. 아래코드를 보자.

```
function sumTotal() {
    return Array.from(arguments).reduce((acc, cur) => acc + cur));
}

sumTotal(1, 2, 3, 4, 5, 6, 7, 8, 9)
```

위 코드는 가변인자 즉 값이 몇개가 들어올지 모르는 값들을 받아서 모두 더해주는 코드이다. arguments객체를 활용하여 가변인자값들을 받아온 것이다. 하지만 위 코드도 문제가 하나 있다. 만약 개발자가 추가적으로 인자를 하나 받고싶다면 어떻게 해야할까 ? 이럴때 Rest Parameters를 사용할 수 있다.

```
function sumTotal(...args) {
    return args.reduce((acc, cur) => acc + cur));
}

sumTotal(1, 2, 3, 4, 5, 6, 7, 8, 9)
```

위 코드처럼 사용하면 된다. ...은 spread와는 다르기 때문에 헷갈림에 유의하고, 바로 배열로 받아오기 때문에 굳이 Array.from으로 형변환 시켜줄 필요가 없다. 자 여기서 만약 값이 추가 된다면 어떻게 하면 될까 ?

```
function sumTotal(bonusValue, ...args) {
    console.log(bonusValue); // 100
    return args.reduce((acc, cur) => acc + cur));
}

sumTotal(100, 1, 2, 3, 4, 5, 6, 7, 8, 9)
```

이 처럼 추가적인 매개변수를 받을 수 있다. 하지만 주의 점은 ...args는 나머지 매개변수기 때문에 항상 마지막에 위치해야 한다. 그리고 하나의 ...만 존재할 수 있다.
