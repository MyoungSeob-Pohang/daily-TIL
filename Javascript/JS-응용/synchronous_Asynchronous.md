# 동기 & 비동기

동기와 비동기는 자바스크립트에서 아주 중요한 개념이다.

만약 수행해야할 작업 즉 함수가 3개가 있다고 가정하자.

```
function TaskA() {
  console.log("TASK A");
}
function TaskB() {
  console.log("TASK B");
}
function TaskC() {
  console.log("TASK C");
}

TaskA();
TaskB();
TaskC();
```

TaskA, TaskB, TaskC 3가지 일을 수행해야 하고 순서는 상관없다고 가정하자.
자바스크립트가 3가지 방식을 처리하는 방법은 아래와 같다.
![Thread](https://github.com/MyoungSeob-Pohang/daily-TIL/blob/main/Javascript/image/JS-Thread.png)

일단은 프로그래밍 된 코드를 계산해서 실행해야 하니 연산과정이 필요할 것이고 그 연산과정은 코드 한줄한줄 모두 수행된다.
이 연산과정을 담당하는 친구가 Thread 쓰레드라고 한다. 스타크래프트로 치면 SCV정도가 된다. 이 쓰레드가 코드를 한줄한줄 수행시킨다.
그래서 이 쓰레드가 TaskA, TaskB, TaskC 차례대로 수행하고 각 시간이 0.3, 0.5, 0.2초가 걸린다고 사진에 되어있다.
이 쓰레드는 TaskA를 실행하는 동안 TaskA에 집중을 하기 때문에 TaskB를 실행하지 않고 A가 끝나야 B를 실행한다. 그리고 B가 끝나야 C를 수행하게 된다.
이렇게 지시한 순서대로 작업하면서 앞의 작업이 끝나야 뒤에 작업을 실행하는 것을 **_동기적 실행 방식 또는 블로킹방식_** 이라고 한다.
내가 실행하고 있으니 넘보지마 ! 라고 하는 것과 같은 것이다. 우리가 지금까지 작성한 코드 모두가 동기적방식을 사용하고 있었던 것이다.

지금은 하나하나의 수행시간이 짧아서 전혀 문제될 것이 없다. 하지만 하나하의 작업의 시간이 길어진다고 하면 이 동기적방식의 문제가 생기게 된다.
![Thread-problem](https://github.com/MyoungSeob-Pohang/daily-TIL/blob/main/Javascript/image/JS-Thread-problem.png)

위 사진을 보면 TaskB는 앞에 작업이 0.3초만에 끝나서 문제가 없어 보이지만 TaskC는 앞에 작업이 20초나 걸리고 TaskC작업도 10초나 걸린다.
총 모든 작업이 끝나려고 하면 30초나 걸리게 된다.
이것은 아주 큰 문제가 된다. 만약 네이버에서 검색하는데 30초가 걸린다고 하면 누가 네이버를 사용할까 ?
이렇게 동기적으로만 처리를 하게 된다면 성능상에 문제가 생기게 된다. 이것이 자바스크립트의 문제점이다. 바로 일꾼이 하나라는 것이다.
다른 언어는 쓰레드를 여러개 사용하는 언어들도 많다(멀티쓰레드 방식). 하지만 자바스크립트는 싱글쓰레드 즉 일꾼이 1개라는 점이다. 그래서 일꾼을 여러개 사용하는 방법이 사용 불가능하다.  
일꾼이 여러마리 있다면 일꾼A에게 TaskA 일꾼B에게 TaskB, 일꾼C에게 TaskC를 준다고 하면 훨씬 빨리 끝나는데 말이다.

그러면 자바스크립트는 정말 방법이 없을까 ? 아니다 방법이 있다. 자바스크립트는 비동기 작업 즉 논 블로킹 방식이라고 하여 하나의 쓰레드에 동시에 여러개 작업을 시켜 버린다.
![Thread-multi](https://github.com/MyoungSeob-Pohang/daily-TIL/blob/main/Javascript/image/JS-Thread-multi.png)

TaskA, TaskB, TaskC가 앞에 작업이 끝나던 말던 상관없이 그냥 동시에 다 실행 시켜버리는 것이다. 이렇게 동시에 작업을 하게 하는 방식을 비동기 방식이라고 한다.
그런데 또 문제가 있다, 이렇게 동시에 작업시켜 버리면 작업이 끝났는지 어떻게 알 수있고 그 결과는 어떻게 받을 수 있을까 ?
이럴때는 비동기로 작업한 A, B, C 각 함수들에게 끝나면 이거 실행해! 라고 콜백함수를 붙여서 전달해주면 된다.

```
TaskA((resultA) => {
  console.log(`A 작업 끝. 결과 : ${resultA}`);
});

TaskB((resultB) => {
  console.log(`B 작업 끝. 결과 : ${resultB}`);
});

TaskC((resultC) => {
  console.log(`C 작업 끝. 결과 : ${resultC}`);
});
```

```
function taskA() {
  setTimeout(() => {
    console.log("A 작업 끝");
  }, 2000);
}

taskA();
console.log("코드 끝");

출력 : 코드 끝
       A 작업 끝 // 2초 후에 실행
```

위 코드를 보면 taskA에 setTimeout이라는 함수를 사용하였다. setTimeout이라는 함수는 타이머를 만들 수 있는 내장 비동기 함수가 존재한다.
첫번째 인자로는 콜백함수를 전달하였고 두번째 인자는 m/s 즉 초를 전달하였다. (1000 m/s === 1초)  
즉 taskA의 setTimeout은 2초 후에 "A 작업 끝" 이라는 문자를 출력하는 함수가 된것이다.

분명히 지시한 순서는 taskA를 먼저 호출하고 코드 끝이라고 출력하였는데 출력된 것을 보면 코드 끝이 먼저 출력되고 taskA가 실행되었다.
즉 taskA(); 의 종료를 기다리지 않고 console.log("코드 끝");가 먼저 실행된 것이다. 이러한 방식이 비동기 방식이다.
만약 taskA에서 결과를 return 받고 싶으면 ? 콜백함수를 사용하면 된다.

```
function taskA(a, b, cb) {
  setTimeout(() => {
    const res = a + b;
    cb(res);
  }, 2000);
}

taskA(3, 4, (res) => {
  console.log(`A TASK RESULT : ${res}`);
});

console.log("코드 끝");

출력 : 코드 끝
       A TASK RESULT : 7
```

위 코드를 보면 taskA 함수에서 a, b 숫자와 콜백함수를 뜻하는 cb를 받아서 지역상수인 res에 a + b 의 연산값을 담아서 cb콜백함수에 결과를 전달하고 있다.
그래서 출력값을 보면 A TASK RESULT : 7 이라는 값을 볼 수 있는 것이다.
이렇게 비동기 처리에 값을 활용해야 할때는 콜백함수를 사용하여 이용할 수 있다.
자 그럼 TaskB도 만들어 보자.

```
function taskA(a, b, cb) {
  setTimeout(() => {
    const res = a + b;
    cb(res);
  }, 2000);
}

function taskB(a, b, cb) {
  setTimeout(() => {
    const res = a * b;
    cb(res);
  }, 1000);
}

taskA(3, 4, (res) => {
  console.log(`A TASK RESULT : ${res}`);
});

taskB(3, 4, (res) => {
  console.log(`B TASK RESULT : ${res}`);
});

console.log("코드 끝");

출력 : 코드 끝
       B TASK RESULT : 12
       A TASK RESULT : 7
```

TaskA와 거의 유사하게 만들었지만 TaskB는 a \* b를 전달한다. 그런데 출력을 보면 알 수 있듯이 TaskB는 1초 후에 실행되는 코드이고 TaskA는 2초 후에 실행되는 코드이므로,
출력값을 보면 코드 끝 -> taskB -> taskA 순서로 실행된 것을 볼 수 있다. 즉 B가 A보다 먼저 실행된 것을 알 수 있다.

## 자바스크립트 작동 원리 ( 동기 방식 )

![JS_Engine](https://github.com/MyoungSeob-Pohang/daily-TIL/blob/main/Javascript/image/heap_stack.png)

자바스크립트로 작성한 코드들은 웹브라우저에 탑재된 자바스크립트 엔진에 의해 해석되고 실행된다.
JS엔진은 Heap, Call Stack 두가지 구성요소로 이루어져 있다.

Heap: 변수나 상수들에 사용되는 메모리 저장 영역
Call Stack : 우리가 작성한 코드의 실행에 따라서 호출스택이라는 것을 쌓는 영역이다.

![Stack](https://github.com/MyoungSeob-Pohang/daily-TIL/blob/main/Javascript/image/stack_1.png)
자바스크립트의 코드가 실행되면 Call Stack에 보이는거 처럼 자바스크립트 코드 최상위 문맥인 Main Context가 가장먼저 Call Stack에 들어간다.
즉 자바스크립트의 Main Context가 Call Stack에 들어가는 순간이 바로 프로그램 실행 순간이고 Main Context가 Call Stack에서 나가는 순간이 프로그램 종료되는 순간이다.

![Stack](https://github.com/MyoungSeob-Pohang/daily-TIL/blob/main/Javascript/image/stack_2.png)
코드의 실행 순서에 따라서 선언된 함수들을 저장 시킨 후 console.log(three())가 실행된다. 이때 three()라는 함수를 실행하게 되니 Call Stack에 three()함수가 추가 된다.

![Stack](https://github.com/MyoungSeob-Pohang/daily-TIL/blob/main/Javascript/image/stack_3.png)
three함수에 들어가보니 two함수를 호출하고 있다, 그래서 two함수도 Call Stack에 추가 된다.

![Stack](https://github.com/MyoungSeob-Pohang/daily-TIL/blob/main/Javascript/image/stack_4.png)
two함수에 가보니 또 one함수를 호출하고 있다, 그래서 one함수도 Call Stack에 추가 된다. 그리고 one 함수는 1을 return 하고 있다. 그럼 one()함수의 할 일은 끝이나게 된다.
이 Call Stack은 할일이 끝난 것은 바로 out 시켜버린다. 그래서 Call Stack에서 one() 함수가 Out 된다.

![Stack](https://github.com/MyoungSeob-Pohang/daily-TIL/blob/main/Javascript/image/stack_5.png)
이 Call Stack에서 사용되는 stack라는 구조는 프링글스 통처럼 가장 마지막에 들어온 것이 가장 먼저 제거되는 방식이다. 그래서 가장 마지막에 호출된 one함수가 가장 먼저 종료되었다. 그럼 똑같이 two 함수에서도 one 함수의 return 값과 + 1 한 값을 three에게 return 해 주니 two함수도 역할이 끝나서 제거가 되고,
three함수도 two에서 받은 값 + 1한 값을 return 하고 three함수도 제거되게 된다. (LIFO : Last in Frist Out / 후입선출)
그러고 나서 console.log가 실행되고 main Context가 마지막으로 제거되면서 프로그램이 종료가 되게 된다. 총 정리하면 아래와 같다.

1. 프로그램 시작 (call stack에 main context 입장)
2. 선언 된 함수 최상위로 올라감
3. console.log()에서 three()를 호출 (call stack에 three() 입장)
4. three()에서 two() 호출 (call stack에 two() 입장)
5. two()에서 one() 호출 (call stack에 one() 입장)
6. one() 에서 1 return (call stack에서 one() 퇴장)
7. two() 에서 1 + 1(2) return (call stack에서 two() 퇴장)
8. three() 에서 2 + 1(3) return (call stack에서 three() 퇴장)
9. console.log(3) 실행
10. call stack에서 main context 퇴장
11. 프로그램 종료

## 자바스크립트 작동 원리 ( 비동기 방식 )

![asynchronous](https://github.com/MyoungSeob-Pohang/daily-TIL/blob/main/Javascript/image/asynchronous_1.png)

동기방식과 다르게 Web APIs 와 Callback Queue, Event Loop라는 새로운 것이 추가되었다.
이 3가지는 자바스크립트 엔진과 웹브라우져의 상호작용 등을 처리하기 위해 존재하는데 가장 대표적인 상호작용이 바로 비동기 처리이다.

![asynchronous](https://github.com/MyoungSeob-Pohang/daily-TIL/blob/main/Javascript/image/asynchronous_2.png)
자 그럼 먼저 동기방식과 동일하게 Main Context가 Call Stack에 올라가게 되고 선언된 함수가 기억되고 asyncAdd() 가 가장 먼저 실행된다.
그렇기 때문에 Call Stack에 asyncAdd()가 추가 된다.
asyncAdd()에 1, 3, 콜백함수를 전달하게 되면 a, b, cb가 그 값을 각각 받아서 setTimeout이라는 비동기함수를 실행하고 있다. 그런데 setTimeout함수 안에는
cb라고 하는 콜백함수 까지 포함하고 있어서 Call Stack에 아래와 같이 추가된다.

![asynchronous](https://github.com/MyoungSeob-Pohang/daily-TIL/blob/main/Javascript/image/asynchronous_3.png)
setTimeout을 빨간색으로 표시한 이유는 setTimeout은 다른 함수들과 달리 비동기함수이기 때문이다. setTimeout함수가 3초를 기다리게 되어있는데 이걸 Call Stack에 그대로 두게 된다면 정말 3초를 기다렸다가 그 다음 코드를 실행해야 한다. 그렇기 때문에 자바스크립트 엔진은 비동기 함수를 Web APIs에 넘겨버린다.

![asynchronous](https://github.com/MyoungSeob-Pohang/daily-TIL/blob/main/Javascript/image/asynchronous_4.png)
자 그러면 Web APIs에 넘겨진 setTimeout은 실행이 멈추는 것이 아니라 그냥 3초 대기를 하게된다.  
이렇게 setTimeout이 Call Stack에 머무르지 않기 때문에 바로 다음 코드를 수행할 수 있게되고 asyncAdd()함수를 끝낼 수 있게된다.  
그럼 asyncAdd() 함수는 Call Stack에서 제거되고 3초를 기다림이 끝났다면 setTimeout은 종료되고 cb(콜백함수)는 수행을 해야 하기 때문에 Callback Queue로 옮겨지게 된다.

![asynchronous](https://github.com/MyoungSeob-Pohang/daily-TIL/blob/main/Javascript/image/asynchronous_5.png)
자 이렇게 Callback Queue로 옮겨진 콜백함수는 Event Loop에 의해 다시 Call Stack로 다시 옮겨질 수 있는데 이 Event Loop는 Call Stack에 Main Context를 제외한 다른 함수가 남아있지 않는지 계속해서 확인한다. 만약 남아있지 않다면 콜백함수를 실행 할 수 있으니 콜백함수를 실행하고 콜백함수는 Call Stack에서 제거된다.
그리고 Main Context가 종료되면서 프로그램이 종료되게 된다. 총 정리하면 아래와 같다.

1. 프로그램 시작 (call stack에 main context 입장)
2. 선언 된 함수 최상위로 올라감
3. asyncAdd()를 호출 (call stack에 asyncAdd() 입장)
4. setTimeout과 cb를 만남
5. setTimeout과 cb를 Web APIs로 넘김
6. asyncAdd() 종료 (call stack에 asyncAdd() 퇴장)
7. 3초의 기다림이 끝난 cb가 Callback Queue로 넘어감
8. Event Loop가 Main Context를 제외한 다른 실행 할 함수가 있는지 확인 (asyncAdd()가 종료되면서 Call Stack에는 main context만 남아있음)
9. cb가 Call Stack로 넘어감
10. Console.log("결과 : ", res); 실행
11. 콜백함수 종료 (call stack에 cb() 퇴장)
12. call stack에서 main context 퇴장
13. 프로그램 종료

## 비동기 처리의 값을 또 다른 비동기처리로 ..

```
function taskA(a, b, cb) {
  setTimeout(() => {
    const res = a + b;
    cb(res);
  }, 3000);
}

function taskB(a, cb) {
  setTimeout(() => {
    const res = a * 2;
    cb(res);
  }, 1000);
}

function taskC(a, cb) {
  setTimeout(() => {
    const res = a * -1;
    cb(res);
  }, 2000);
}

taskA(4, 5, (a_res) => {
  console.log(`A TASK RESULT : ${a_res}`);
  taskB(a_res, (b_res) => {
    console.log(`B TASK RESULT : ${b_res}`);
    taskC(b_res, (c_res) => {
      console.log(`C TASK RESULT : ${c_res}`);
    });
  });
});

console.log("코드 끝");

출력 : 코드 끝
        A TASK RESULT : 9
        B TASK RESULT : 18
        C TASK RESULT : -18
```

위 코드를 보면 taskA에서 계산된 결과(4 + 5 = 9)를 출력(3초) 후 그 값 즉 a*res를 taskB에 넘겨주고 있다.
taskB는 a_res를 받아 * 2 한 값 (9 _ 2 = 18)을 출력하고(1초) 다시 그 값 즉 b_res를 taskC에 넘겨주고 있다.
taskC는 b_res를 받아 _ -1 한 값 (18 \_ -1 = -18)을 출력(2초)하고 있다.
콜백함수에 콜백함수에 콜백함수를 넣어서 비동기처리의 값을 또다른 비동기처리에 넘겨줄 수 있다.
실제로 이런 비동기 함수의 값을 다른 비동기 처리에 넘겨주는 경우가 많다. 그런데 딱 봐도 가독성이 좋지않다. 만약 task가 20개라면 ?
코드는 계속 안으로 파고들게 될 것이다. 아래와 같은 짤이 인터넷에 돌아다닐 정도이니 .. 이런 코드를 콜백지옥 이라고 한다.  
![Hell](https://github.com/MyoungSeob-Pohang/daily-TIL/blob/main/Javascript/image/hell.png)

다음 문서는 Callback지옥을 벋어나기 위해 자바스크립트의 비동기 담당 Promise객체가 있다.

[Promise](https://github.com/MyoungSeob-Pohang/daily-TIL/blob/main/Javascript/JS-%EC%9D%91%EC%9A%A9/promise.md)
