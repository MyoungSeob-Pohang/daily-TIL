# 반복문

프로그래밍은 조건과 반복에 의해 완성된다는 말이 있을 정도로 반복문은 프로그래밍에 있어 절때 빠질 수 없는 요소이다.
반복문은 특정명령을 반복해서 수행해야 할 때 사용한다. 만약 본인의 이름을 console에 40번 출력해야 한다면 console.log()를 40번 써야 한다.
하지만 반복문을 사용하면 40줄이 아니라 아주 짧은 코드로 100번이든 1000번이든 10만번이든 반복할 수 있다.

## for문

```
for(let i = 0; i < 100; i++){
    // 반복할 코드
    console.log("moonsbi");
};
```

하나하나 코드를 뜯어보자.
for : for문 선언

(let i = 0; i < 100; i++) : ;을 기준으로 let i = 0 이 부분은 변수선언 부분, i < 100; 반복할 횟수, i++ 증가 이다.
즉 i라는 변수를 선언해서 그 i 가 100미만이 될때 까지 1씩 증가하면서 라는 뜻이된다. 그리고 {} 중괄호 안에 반복할 코드를 넣는다.

순서를 따져보면

1. i 라는 변수가 선언되고 0이라는 값을 가지게 된다.
2. i 가 100 미만인지 확인한다.
3. 반복할 코드 즉 console.log("moonsbi"); 를 실행한다.
4. i++ 즉 i를 1 증가 시킨다.
5. 2번으로 돌아가서 i가 100미만인지 확인한다.
6. 반복할 코드 즉 console.log("moonsbi"); 를 실행한다.
7. i++ 즉 i를 1 증가 시킨다.
8.  5. 2번으로 돌아가서 i가 100미만인지 확인한다.......

이렇게 i 가 100이 될 때 까지 반복하는 코드이다.

특히 for문은 배열과 같이 쓸때 유용한데,

```
let arr = [1, 2, 3, 4, 5];

for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]);
}
```

똑같이 진행된다. 지금 arr 배열의 길이는 5이다. 5번 반복하는데 인덱스 넘버이니 i를 0으로 선언하였다.
그리고 arr[i]번째 즉 0 부터 1씩 증가하여 배열의 길이인 5미만인 4까지 진행하여 출력한다.
1, 2, 3, 4, 5가 출력이 된다.

객체도 배열과 동일하게 접근이 가능하다.

```
let person = {
  name: "moonsbi",
  age: 25,
  height: 199
};

// 객체의 key 값만 받아오기
const personKey = Object.keys(person);

for (let i = 0; i < personKey.length; i++) {
  console.log(personKey[i]);
}

// 객체의 value값만 가져오기
const personValue = Object.values(person);

for (let i = 0; i < personValue.length; i++) {
  console.log(personValue[i]);
}

// key, value 둘다 가져오기

for (let i = 0; i < personKey.length; i++) {
  const curKey = personKey[i];
  const curValue = person[curKey];

  console.log(`${curKey} : ${curValue}`);
}
```

## 다른 반복문

Link: [반복문 총정리: for in, for of, forEach 등][https://curryyou.tistory.com/202]
