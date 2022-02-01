# async / await | 직관적인 비동기 처리

Pormise를 좀더 쉽고 가독성 좋게 작성할 수 있는 async / await을 알아보자.

## async

async도 Promise처럼 비동기를 다루는 기능이고 더 쉽게 도와준다.

간단한 코드로 알아보자.

```
function hello() {
  return "hello";
}

async function helloAsync() {
  return "hello Async";
}

console.log(hello());
console.log(helloAsync());

출력 :
        hello
        Promise {<pending>}
```

지금보면 hello함수의 값은 hello가 잘 출력되었는데,  
helloAsync함수는 앞에 async가 붙어 있고 그 함수를 호출하니 Promise Pending(대기) 상태를 출력했다.  
이게 무슨뜻이냐면 Promise를 그냥 출력하게 되면 Promise가 그대로 출력된다. 그러니까 helloAsync()에서는 Promise를 리턴하고 있다 라고 생각하면 된다.  
이렇게 async를 붙여주게 되면 그 함수는 자동적으로 Promise를 리턴하는 비동기처리 함수가 된다.

그리고 Promise를 return하기 때문에 당연히 then도 사용할 수 있다.

```
async function helloAsync() {
  return "hello Async";
}

helloAsync().then((res) => {
  console.log(res);
});

출력 : hello Async
```

helloAsync()의 return값인 hello Async가 res에 전달되었고 그것을 그대로 출력한 코드이다.  
이렇게 async키워드를 붙여준 함수의 return값은 비동기객체 Promise의 reslove 결과값이 된다.  
이렇게 async 붙이고 return만 해도 Promise와 동일한 결과값을 얻는다 라는 결과를 도출할 수 있다.

## await

await키워드를 비동기함수의 호출앞에 붙이게 되면 비동기 함수가 마치 동기적인 함수처럼 작동하게 된다. 즉  
await키워드가 붙은 함수의 호출은 뒤에있는 코드가 완료될때 까지 그 아래에 있는 코드를 수행하지 않게 된다. 결론 await줄에 있는 애들은 다 동기적으로 사용되는 것이다.

```
function delay(ms) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      return resolve();
    }, ms);
  });
}

async function helloAsync() {
  await delay(3000);
  return "hello async";
}

helloAsync().then((res) => {
  console.log(res);
});

출력 : hello async
```

위 코드를 보면 hello async라는 출력을 받기전에 3초 딜레이를 주고 싶어서 delay함수를 만들었다. delay함수는 ms(미리세컨트)를 받아서 setTimeout을 실행 후 reslove()를 호출한다.  
async helloAsync()함수에서 await delay(3000)을 만났으니 delay함수는 동기적으로 사용되기 때문에 3초 후에 hello async가 출력되게 된다.
이렇게 await는 동기적인 수행을 하게된다. 그리고 await는 async키워드가 붙은 함수내에서만 사용할 수 있다.

helloAsync의 값도 await를 사용하여 받아올 수 있는데 , 아래코드와 같다.

```
function delay(ms) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      return resolve();
    }, ms);
  });
}

async function helloAsync() {
  await delay(3000);
  return "hello async";
}

async function main() {
  const res = await helloAsync();
  console.log(res);
}

main();

출력 : hello async
```

main()에서 helloAsync()를 await를 사용하여 동기적으로 수행 한 후 그 값을 상수 res에 담아서 출력하였다.
