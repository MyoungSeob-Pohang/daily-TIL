# 복잡한 인자 관리하기

자바스크립트 에서 매개변수를 다룰 때 2개 이상, 3개 이상, 4개 이상이 많다 라는 여러 얘기들이 있지만, 개인적으로는 맥락에 어울리면 된다고 생각한다. 3개 4개가 넘으면 관리가 어렵겠지만 갯수별로 맥락을 유추해 볼 수 있다면 크게 문제가 없다고 생각한다.

```
function toggleDisplay(isToggle)
function sum(sum1, sum2)
function genRandomNumber(min, max)
function timer(start, stop, end)
function genSquare(top, right, bottom, left)
```

위 함수들 모두 매개변수와 함수명을 보면 대충 어느정도 무슨일을 하는 함수인지 유추해볼 수 있다. 저렇게 예측이 가능할 때에는 매개변수가 몇개가 있든 상관이 없다고 생각한다.

```
function createCar(name, brand, color, type){
    return {
        name,
        brand,
        color,
        type
    }
}
```

위 코드 같은 경우 유추하기가 어렵다, 그렇기 때문에 객체 구조 분해할당을 사용하는것이 가장 좋다. 가장 중요한 값은 구조 분해할당을 하지 않거나 아니면 값을 받아왔을 때 없다면 바로 new Error로 에러를 throw해줄 수도 있다.
