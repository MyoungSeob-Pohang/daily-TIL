# 배열 메서드 체이닝 활용하기

```
const price = ['2000', '1000', '3000', '5000', '4000'];

const suffixWon = (price) => price + '원';
const isOverOneThousand = (price) = Number(price) > 1000;
const ascendingList = (a, b) => a - b;

function getWonPrice(priceList) {
    const isOverList = priceList.filter((price) => Number(price) > 1000);
    const sortList = isOverList.sort(ascendingList);

    return sortList.map(suffixWon);
}

const result = getWonPrice(price);
```

위 코드는 배열메서드를 활용한 코드이다. filter, sort, map을 활용하여 좋은 코드가 되었는데, 아쉬움이 조금 남는다. 만약 조건들이 더 늘어난다면 위 코드들도 복잡해 질 수 있다는 것이다. 이럴 때 메서드 체이닝을 사용하면 해결할 수 있다. 체이닝이란게 어려워 보이지만 별거없다.

```
const price = ['2000', '1000', '3000', '5000', '4000'];

const suffixWon = (price) => price + '원';
const isOverOneThousand = (price) = Number(price) > 1000;
const ascendingList = (a, b) => a - b;

function getWonPrice(priceList) {
    return priceList
        .filter(isOverOneThousand)
        .sort(ascendingList)
        .map(suffixWon);
}

const result = getWonPrice(price);
```

지금 보면 매개변수로 받아온 priceList에 filter를 거치고 sort 정렬을 한다음 map으로 새로운 배열로 반환했다. 세번 나누어진 것을 그냥 하나로 합친것이다. 결과값은 당연히 동일하게 나온다. 딱 코드를 보기만 해도 해석하기 쉽고 읽기 쉽다.
