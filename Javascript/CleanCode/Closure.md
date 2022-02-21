# Closure

mdn에서 클로저를 아래와 같이 정의한다.

```
"A closure is the combination of a function bundled together (enclosed) with references to its surrounding state (the lexical environment). In other words, a closure gives you access to an outer function’s scope from an inner function. In JavaScript, closures are created every time a function is created, at function creation time."

클로저는 주변의 상태 (lexical environment)의 참조와 함께 번들로 묶인 함수의 조합입니다. 즉, 클로져는 우리에게 inner함수에서 outer함수의 스코프에 접근을 가능하게 해줍니다. 자바스크립트에서 클로저는 함수가 생성될 때마다 생성됩니다.
```

말만 보면 무슨말인가 싶기도 하다. 간단하게 설명하자면 어떤 함수가 자신의 내부가 아닌 외부에서 선언된 변수에 접근하는 것을 말한다. 코드로 보자

```
function add(num1){
    return function(num2) {
       return num1 + num2;
    }
}

const addOne = add(1); // ƒ () {}
const addTwo = add(2); // ƒ () {}

console.log(addOne(3)); // 4
console.log(addTwo(5)); // 7
```

코드를 보면 혼란스러울 수 있다. 지금 보면 addOne은 외부 함수 add만 실행 된 것이고 대신 실행된 상태에서 내부에 있는 return function도 기억을 하고 있는 것이다. addOne의 출력을 보면 함수를 가지고 있다. 즉 1이라는 값을 넘긴 상태로 내부 scope를 기억해둔 것이니 console.log(addOne(3));이렇게 호출하면 1 + 3 인 4가 출력이 되는 것이다. addTwo도 보면 2를 넘긴 상태에서 내부 scope 를 기억해둔 것이고 5를 넘겻으니 둘을 합한 7값이 나온 것이다. 똑같은 함수를 실행시킬 때 마다 개별 Context를 기억하고 있는 것이다.

```
function add(num1) {
    return function(num2) {
        return function (calculateFn) {
            return calculateFn(num1, num2);
        }
    }
}

function sum(num1, num2) {
    return num1 + num2;
}

const addOne = add(1)(2);
const sumAdd = addOne(sum);
```

위 코드를 보자 addOne을 선언하면서 add(1)(2)를 할당하였다. 이 1과 2는 add(num1) function(num2)에 각각 저장 되고 function (calculateFn)를 기억하고있다. 그래서 addOne를 콘솔에 출력하면 ƒ () {} 함수가 출력된다. 그럼 addOne가 가진 값음 1, 2, function 이다. 그 상태에서 sumAdd = addOne(sum) addOne에 sum 함수를 넣어주면 1과 2가 더한 결과가 리턴되면서 결국 3이 출력되게 된다. 각자의 context 영역을 가지고 있는 것이다. 이렇게 closure를 사용할 수 있다.

Closure는 더욱 더 이해가 필요할것 같다.
