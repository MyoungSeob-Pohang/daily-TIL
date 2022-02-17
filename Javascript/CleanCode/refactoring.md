# for문 배열 고차 함수로 리팩터링

이전 문서에서 임시변수를 줄여야 한다고 강조해서 얘기를 하였다. 실제로 많은 사람들이 for문을 작성할 때 아래와 같은 코드를 사용한다.

```
const price = ['2000', '1000', '3000', '5000', '4000'];

function getWonPrice(priceList) {
    let temp = [];

    for(let i = 0; i < priceList.length; i++) {
        temp.push(priceList[i] + '원');
    }

    return temp;
}
```

이 때 배열고차함수를 사용해서 조금 더 선언적으로 , 명시적으로 바꿀 수 있는데 바로 MAP이라는 배열매서드를 활용하는 것이다.

```
const price = ['2000', '1000', '3000', '5000', '4000'];

function getWonPrice(priceList) {
    return priceList.map((price) => price + '원');
}
```

불필요한 임수변수인 temp도 삭제되었고, for문을 사용하기 위한 i같은 의미없는 값도 사라졌고 코드도 짧아졌으며 더 명시적으로 코드가 변했다. 결과는 동일한데 말이다.  
그리고 추가적인 요구사항이 생긴다고 하여도 더 손쉽게 코드를 작성하기가 쉽다. 만약 1000원 이상의 값만 뽑으라고 한다면,

```
const price = ['2000', '1000', '3000', '5000', '4000'];

function getWonPrice(priceList) {
    const isOverList = priceList.filter((price) => Number(price) > 1000);

    return isOverList.map((price) => price + '원');
}
```

filter를 작성하여 1000원보다 큰 아이들로 새로운 객채를 받고 map을 통해서 그 값에 원을 붙여주고 출력하였다. 이런식으로 코드가 읽기도 편해지고 확장하기도 편해지기 때문에 map, filter과 같은 배열 고차함수를 잘 배우고 쓰는게 좋다.
