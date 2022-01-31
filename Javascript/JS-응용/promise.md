# Promise | 콜백지옥 탈출

동기/비동기문서에서 콜백지옥에 대해서 경험하였다. 그 콜백지옥을 어떻게 탈출 할 수 있는지 알아보자.  
이런 콜백지옥에서 탈출할 수 있게 해주는 자바스크립트 Promise객체가 있다.  
Promise는 자바스크립트의 비동기를 돕는 객체이다. 간단히 말하면 Promise를 사용하면 이제 더이상 콜백을 줄지어서 사용할 필요가 없다.

일단 비동기작업이 가질 수 있는 3가지 상태를 알아보자.

1. 대기상태(Pending) : 비동기 상태가 작업중이거나 시작할 수 없는 문제가 발생한 경우
2. 성공 (Fulfilled) : 이행, 또는 성공 상태. 비동기 상태가 정상적으로 진행된 상태
3. 실패 (Rejected) : 버그, 또는 실패한 상태로 비동기 작업이 정상적으로 진행되지 않은 상태

대기상태에서 성공 상태가 되는 것은 resolve(리졸브) 실패 상태가 되는 것을 reject(리젝트)라고 한다.

코드로 확인해보기 위해, 2초뒤에 전달 받은 값이 number형인지 확인하고 그 값이 양수인지 음수인지 확인하는 함수를 하나 만들어보자.

```
function isPositive(number, resolve, reject) {
  setTimeout(() => {
    if (typeof number === "number") {
      // 성공 -> resolve
      resolve(number >= 0 ? "양수" : "음수");
    } else {
      // 실패 -> reject
      reject("주어진 값이 숫자형 값이 아닙니다.");
    }
  }, 2000);
}

isPositive(
  10,
  (res) => {
    console.log(`성공적으로 실행 : ${res}`);
  },
  (err) => {
    console.log(`실패 : ${err}`);
  }
);

출력 : 성공적으로 실행 : 양수
```

지금보면 isPositive()를 호출하면서 값, 성공콜백(resolve), 실패콜백(reject)를 넘겨주고 있다.  
isPositive 함수에서는 setTimeout을 사용하여 비동기로 처리를 하고 number형인지 확인 후에 number형이면 양수인지 음수인지 확인하여 resolve()콜백함수를 실행한다.  
number형이 아니게 되면 reject() 콜백함수를 실행하여 주어진 값이 숫자형이 아니라고 표현하고 있다.

그럼 Promise를 사용법을 알아보자.

```
function isPositiveP(number) {
  const executor = (resolve, reject) => {
    setTimeout(() => {
      if (typeof number === "number") {
        resolve(number >= 0 ? "양수" : "음수");
      } else {
        reject("주어진 값이 숫자형 값이 아닙니다.");
      }
    }, 2000);
  };

  const asyncTask = new Promise(executor);
  return asyncTask;
}

const res = isPositiveP(101);
res
  .then((res) => {
    console.log(`작업 성공 : ${res}`);
  })
  .catch((err) => {
    console.log(`작업 실패 : ${err}`);
  });
```

새로운 함수 isPositiveP()를 만들었다. 그렌데 const executor이라는 상수에서 resolve, reject를 가지고 setTimeout 을 실행하고 있다.  
executor() 는 해석하자면 실행자 / 집행자라는 뜻이다. executor()는 비동기 작업을 실제로 수행하는 친구이다 라고 생각하면 된다.  
executor는 resolve : 비동기 작업이 성공했을 때의 콜백함수, reject : 비동기 작업이 실패했을 때의 콜백함수를 가지고 있다.  
그리고 아래 코드를 보면 const asyncTask = new Promise(executor); 라고 new를 사용하여 Promise를 만들어서 executor()를 전달하고 있다.  
asyncTask에 Promise객체의 생성자로 비동기 작업의 실질적인 실행자 함수인 executor를 넘겨주게 되면 자동으로 executor함수가 자동으로 실행되게 된다.  
자 정리하자면 executor를 담은 Promise객체를 asyncTask인 상수에 담아서 asyncTask를 리턴하고 있다. asyncTask는 Promise 객체이다.  
자 이렇게 상수를 리턴하게 되면 const res상수가 Promise객체를 리턴받게 되고 res가 가진 Promise객체를 가지고 then과 catch를 통해서 resolve와 reject의 결과값을 아무대서나 사용할 수 있다.

```
res
  .then((res) => {
    console.log(`작업 성공 : ${res}`);
  })
  .catch((err) => {
    console.log(`작업 실패 : ${err}`);
  });
```

이 코드를 보면 res.then으로 resolve 콜백함수의 값을 받아 실행하고 있고, catch에서 reject 콜백함수의 값을 받아 실행하고 있다.
실행순서를 보면, 아래와 같다.

1. isPositiveP() 함수를 실행하면서 101을 전달
2. executor 상수에서 resolve, reject콜백함수를 가지고 setTimeout 실행
3. 전달받은 101의 타입이 number 인지 확인
4. if문에서 조건을 확인하여 "양수"라는 값을 가지고 있으니 resolve()를 실행하면서 "양수" 전달
5. res.then에서 resolve를 받아 작업 성공 : 양수 출력
6. 프로그램 종료

## Promise로 콜백지옥 탈출

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

taskA(3, 4, (a_res) => {
  console.log(`A TASK : ${a_res}`);
  taskB(a_res, (b_res) => {
    console.log(`B TASK : ${b_res}`);
    taskC(b_res, (c_res) => {
      console.log(`C TASK : ${c_res}`);
    });
  });
});
```

위 코드의 콜백지옥을 Promise를 사용하여 콜백지옥을 탈출해보도록 하자.

```
function taskA(a, b) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const res = a + b;
      resolve(res);
    }, 3000);
  });
}

function taskB(a) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const res = a * 2;
      resolve(res);
    }, 1000);
  });
}

function taskC(a) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const res = a * -1;
      resolve(res);
    }, 2000);
  });
}

taskA(5, 1)
  .then((a_res) => {
    console.log(`A RESULT : ${a_res}`);
    return taskB(a_res);
  })
  .then((b_res) => {
    console.log(`B RESULT : ${b_res}`);
    return taskC(b_res);
  })
  .then((c_res) => {
    console.log(`C RESULT : ${c_res}`);
  });
```

**_ Promise가 만들어 질때는 우리가 전달한 executor라는 함수가 바로 실행된다 _**

자 일단 각 함수를 보면 이제 Promise객체의 resolve와 reject를 사용하기 때문에 cb 콜백함수가 필요가 없다. 삭제한 후 return으로 setTimeout안의 비동기 작업을 완료 후 값을 Promise의 객체를 반환한다.

순서대로 차례대로 따라가보자 .

1. taskA함수를 호출하면서 5와 1을 넘겨준다
2. taskA함수는 setTimeout을 통해 3초 후에 5 + 1의 값을 const res 상수에 담아서 resolve() 콜백함수에 전달한다.
3. taskA(5, 1)의 실행이 정상적으로 실행되었기 때문에 reslove가 a_res에 담기게 된다.
4. a_res를 출력 후 return taskB(a_res)를 실행한다. 이때 taskA에서 return하는 값은 taskB를 호출하여 a_res를 넘겨주고 Promise반환 받기 때문에 Promise객체를 반환한다.

```
taskA(5, 1)
  .then((a_res) => {
    console.log(`A RESULT : ${a_res}`);
    return taskB(a_res);
  })
```

이 코드를 자세히 살펴보자 taskA에 5와 1을 넘겨주고 받아온 값은 taskA함수에서 리턴받은 return new Promise 즉 Promise이다.  
그러니 then으로 a_res(5 + 1 = 6)을 받아서 그 결과를 출력하고 있다. 그리고 나서 return 하는데 taskB를 호출하고 6을 넘겨주었다.  
taskB함수에서도 리턴받은 값은 Promise이니 다시 그걸 then으로 엮어서 b의 값을 출력하고 다시 taskC를 호출하면서 6 \* 2 즉 12를 값으로 넘겨준다.  
taskC에서도 리턴받은 값은 Promise이고 그 값을 출력하고 있다.

이렇게 then을 사용하여 아래로 엮어서 나가는 것을 then chaining 이라고 한다. 훨씬 더 직관적이며 코드가 계속 파고들어가지 않아 가시성도 좋다.  
그리고 또 큰 장점은 then에서 끊은 다음 다른 코드를 중간에 삽입할 수 도 있다는 것이다.

```
const bPromiseResult = taskA(5, 1).then((a_res) => {
  console.log(`A RESULT : ${a_res}`);
  return taskB(a_res);
});

console.log("다른작업 1");
console.log("다른작업 2");
console.log("다른작업 3");
console.log("다른작업 4");

bPromiseResult
  .then((b_res) => {
    console.log(`B RESULT : ${b_res}`);
    return taskC(b_res);
  })
  .then((c_res) => {
    console.log(`C RESULT : ${c_res}`);
  });
```

지금 보면 bPromiseResult 상수에 taskA값을 담은 후 다른 작업을 수행하고 다시 bPromiseResult으로 then chaining을 할 수 있다.
