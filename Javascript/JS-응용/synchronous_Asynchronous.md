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
