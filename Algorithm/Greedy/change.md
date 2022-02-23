# 거스름 돈

## 문제

Q. 당신은 음식점의 계산을 도와주는 점원이다. 카운터에는 거스름돈으로 사용할 500원, 100원, 50원, 10원짜리 동전이 무한히 존재한다고 가정한다. 손님에게 거슬러 줘야 할 돈이 N원일 때 거슬러 줘야 할 동전의 최소 개수를 구하라. 단 거슬러 줘야 할 돈 N은 항상 10의 배수이다.

## 풀이

최소 개수를 구하려고 하면 가장 큰 화폐 단위인 500원 부터 거슬러 줄 수 있을만큼 거슬러 주고 100원, 50원, 10원 순서대로 진행하면 된다. 만약 거슬러줘야 할 돈이 1260원 이라면 500원 2개, 100원 2개, 50원 1개, 10원 1개 총 6개의 동전으로 거슬러 줄 수 있다.

```
let input = parseInt(prompt('거슬러 줄 돈'), 10);
let coin = [500, 100, 50, 10];
let count = 0;

coin.forEach((it) => {
    count += parseInt(input / it, 10);
    input %= it;
});

console.log(count);
```

사용언어 JS

1. 사용자에게 거슬러 줄 돈을 받아온다.
2. 코인은 고정적이니 배열로 가장 큰 수부터 나열 한다.
3. forEach를 돌면서 count에는 입력받은 값을 배열의 각 항목과 나누어 몫을 저장한다.
4. 입력받은 값을 나눈 나머지를 input에 저장한다.

## 예시

1. 1260원 입력시 input은 1260
2. coin.forEach의 첫번째 it값은 500, 0 = 0 + (1260 / 500) 즉 count의 값은 2가 됨.
3. 1260 = 1260 % 500 즉 input의 값은 260이 됨.
4. 두번째 it값은 100, 2 = 2 + (260 / 100) 그럼 count값은 총 4가 됨
5. 260 = 260 % 100 즉 input의 값은 60
6. 세번째 it값은 50, 4 = 4 + (60 / 50) 즉 5가 됨
7. 60 = 60 % 50 input은 10이 됨
8. 5 = 5 + 10 / 10 으로 6이 됨
9. 10 / 10 이 됨으로 input의 값은 1
10. coin의 값을 모두 순회 하였으니 forEach 종료
11. 6 출력