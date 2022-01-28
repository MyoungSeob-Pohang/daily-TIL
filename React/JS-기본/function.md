# 함수

프로그래밍의 시작과 끝, 함수
함수는 쉽게 설명해서 input을 받아서 내부적으로 처리를 하거나 혹은 그 결과를 output으로 내보내는 것이다.

만약 직사각형의 넓이를 계산해야 하는 코드를 작성한다고 하면 이제까지 배운대로 아래와 같이 코드를 작성할 수 있다.

```
let width1 = 10;
let height1 = 20;
let area1 = width1 * height1;
console.log(area1); // 200

let width2 = 30;
let height2 = 50;
let area2 = width2 * height2;
console.log(area2); // 1500
```

근데 만약 직사각형이 30개라면 어떨까 ? 저런 코드를 30개를 적어서 계산해야 한다.
하지만 함수를 만들게 되면 아주 짧은 코드로 30개든 300개든 계산을 쉽게할 수 있다.

```
function RectangularArea(width, height) {
  let area = width * height;
  console.log(area);
}

RectangularArea(10, 20); // 200
RectangularArea(30, 50); // 1500
RectangularArea(120, 10); // 1200
RectangularArea(111, 222); // 24642
```

한줄씩 해석해보도록 하자.
function : 함수선언식
RectangularArea : 함수명
(width, height) : 받을 변수 / 매개변수

즉 나는 RectangularArea라는 이름을 가진 함수를 선언할껀데 width와 height를 받을꺼야 ! 라는 뜻이다.

하지만 함수만 만들었다고 출력이 되거나 어떠한 기능을 하지않는다. 만들었으면 호출해서 사용해야 한다.
호출은 RectangularArea(10, 20); 이런식으로 한다.
RectangularArea : 함수명
(10, 20) : 넘길 값 10이 width로 들어가고 20이 height로 들어간다. / 인자

지금은 함수내에서 console.log로 값을 출력하고 있지만 출력하지않고 그 값을 return(output)으로 다시 꺼내서 변수나 상수에 저장하여 사용할 수 있다.

```
function RectangularArea(width, height) {
  let area = width * height;
  return area;
}

let a = RectangularArea(10, 20);
let b = RectangularArea(30, 50);
let c = RectangularArea(120, 10);
let d = RectangularArea(111, 222);
const r = RectangularArea(22, 33);

console.log(a); // 200
console.log(b); // 1500
console.log(c); // 1200
console.log(d); // 24642
console.log(r); // 726
```

그리고 let area = width \* height; 이 부분의 변수 area는 function 안에서만 사용가능한 **_지역변수_** 이다.
지역변수라는 것은 특정한 지역안에서만 사용할 수 있는 변수이고 그 변수를 벋어나게 된다면 사용이 불가능하다.
만약 area라는 변수를 전체 다 쓰고 싶다라고 한다면 함수 밖에 선언해야 한다. 그럼 어디서든 그 변수를 사용가능한데 이를 **_전역변수_** 라고 한다.

```
let c = 20;

function getArea() {
  let area = 10;
  let mul = area * c;
  return mul;
}

let result = getArea();
console.log(result); // 200 출력
console.log(c)); // 20 출력
console.log(area); // Error
```

위 코드를 보면 c는 함수안에서도 사용하고 함수 밖에서도 출력하고 있지만 문제가 없다.
하지만 area변수는 함수안에서 사용되어서 지역변수가 되어버렸기 때문에 함수 밖에서는 호출할 수 없다.
