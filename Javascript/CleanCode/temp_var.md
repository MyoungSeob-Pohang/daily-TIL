# 임시변수 제거하기

자바스크립트를 다룰 때 전역공간을 사용하지말자, var를 사용하지 말자라고 하였다.
근데 임시변수는 무엇일까 ? 임시변수는 특정 공간안에서 전역변수처럼 사용되는 변수이다.

```
function getObject() {
  const result = {};

  result.title = document.querySelector(".title");
  result.text = document.querySelector(".text");
  result.value = document.querySelector(".value");

  return result;
}
```

지금보면 const result = {}; 이게 바로 임시변수이다. 어찌 보면 const라서 상관없는거 아니야 ? 라고 생각할 수 있지만 이 임시객체도 함수가 커지면 전역공간이나 다른 없는 상황이 나올수가 있다. 그러면 이 임시변수가 굉장히 위험해 지는 것이다. 실질적으로 이 임시변수는 나는 굉장히 많이 사용했다.  
임시변수를 선언하고 그 안에 값을 넣거나 연산을 하거나 해서 최종결과를 리턴하거나 출력하는 그러한 코드를 많이 썻는데 굉장히 않좋은 습관이였다.  
그럼 이 임시변수나 객체를 어떻게 접근해서 사용할까 ? 가장 간단한 방법은 함수를 쪼개는 것이 가장 좋은데 위 코드의 경우처럼 임시객체를 만들어서, 그 임시객체에 닷(.)연산자를 사용해서 자료를 계속 추가하고 있다. 근데 저렇게 할 필요가 없는 것이다. 실질적으로 querySelector가져온 값을 객체에 넣어주고 반환하는 함수이기 때문에 아래와 같은 코드로도 가능하다.

```
function getObject() {
  return {
    title : document.querySelector(".title"),
    text : document.querySelector(".text"),
    value : document.querySelector(".value"),
  }
}
```

임시변수, 객체가 생기는 순간 , 이 변수나 객체에 접근해서 조작하고 싶은 생각이 들기 때문에 그런 생각이 들지 않게 끔 함수는 하나의 행동만 할 수 있게끔 작성하는 것이 좋다.

또 하나의 예를 보자,

```
function getDateTime(targetDate) {
  let month = targetDate.getMonth();
  let day = targetDate.getDate();
  let hour = targetDate.Hours();

  month = month >= 10 ? month : '0' + month;
  day = day >= 10 ? day : '0' + day;
  hour = hour >= 10 ? hour : '0' + hour;

  return {month, day, hour}
}
```

위 함수는 특정 Date오브젝트를 받아서 그 오브젝트에서 월 날짜 시간을 뽑아서 그 데이터를 조작 하고 있다.
그런데 만약 이 함수가 할 수 없는 추가적인 스펙이 생기면 어떻게 될까?  
기획/마케팅적 요소로 인해서 날짜에 대한 요구사항이 생겼을 때 요구사항을 위해서 개발자는 함수를 추가로 만들까 아니면 기존 함수를 수정보수 할까 라는 두가지 방향이 생기는데 만약 함수를 수정한다고 하면 저 함수를 쓰는 곳이 한군데라면 상관없지만 보통 프로젝트에서 10군데 혹은 100군데 사용을 하는데 이 함수를 수정하게 된다면 그 모든 부분에 영향이 가게 된다. 그래서 위 코드도 바로 반환할 수 있는 형태로 바꿔주는게 좋다.

```
function getDateTime(targetDate) {
  const month = targetDate.getMonth();
  const day = targetDate.getDate();
  const hour = targetDate.Hours();

  return {
    month : month >= 10 ? month : '0' + month,
    day : day >= 10 ? day : '0' + day,
    hour : hour >= 10 ? hour : '0' + hour,
  }
}
```

let으로 선언했다는 건 수정&재할당한다 라는 약속을 의미할 수 있기 떄문에, 모두 바꿔 버리고 바로 리턴하여 준다.  
그럼 만약 추가 수정 사항이 생겼을 때 어떻게 해야 하냐, 이 함수를 한번더 사용하면 된다.

```
function getDateTime(targetDate) {
  const month = targetDate.getMonth();
  const day = targetDate.getDate();
  const hour = targetDate.Hours();

  return {
    month : month >= 10 ? month : '0' + month,
    day : day >= 10 ? day : '0' + day,
    hour : hour >= 10 ? hour : '0' + hour,
  }
}

function getDateTime(){
  const currentDateTime = getDateTime(new Date());

  return {
    month : `${currentDateTime.month}분 전`,
    day : `${currentDateTime.day}`,
    hour : `${currentDateTime.hour}`,
  }
}
```

currentDateTime에서 기존 함수의 값을 들고와서 조작한 값을 다시 리턴하고 있다. 이렇게 함수를 한번더 추상화 했기 때문에 재활용을 할 수 있다.  
만약 임시변수를 사용했다면 유혹에 이끌려 계속 내부에서 조작하게 되었을 것이다. 따라서 함수를 만들 때 그 누구도 함수 내부의 임시변수 속에서 유혹을 받지 못하도록 단 하나의 역할만 할 수 있는 함수 그런 함수로 만들어 주는게 가장 좋다.

```
function getSomeValue(params) {
  let tempVal = ''

  for(let i = 0; i < array.length; i++) {
    temp = array[i];
    temp += array[i];
    temp += array[i];
    temp += array[i];
  }

  if(temp ??){
    tempVal = ??
  } else if (temp ??){
    temp += ??
  }

  return temp;
}
```

이 코드를 보면 아주 혼란하다. 하지만 나는 지금까지 이러한 형태로 코드작성을 많이 하였다. 완전 명령에 가까운 코드이다. tempVal 이 하나의 임시변수가 함수가 동작하는 동안 여기저기서 바뀌고 연산도 하는데 최종적으로 tempVal이 무슨값인지 예측하기가 어렵다. 그래서 이 임시변수를 사용하지 않는 방법을 많이 고민해야한다.

그래서 요약해보면,

1. 임시변수 제거 - 명령형으로 가득한 코드, 로직이 나온다
2. 어디서 어떻게 잘못되었는지 디버깅이 어렵다.
3. 추가적인 코드를 작성하고 싶은 유혹에 빠지기 쉽다. 결국 유혹당하면 코드 유지보수가 어렵고 하나의 함수가 하나 이상의 일을 하게 되어 알아보기 힘들다.

해결책은

1. 함수를 많이 나눈다.
2. 바로 반환한다
3. 고차함수를 사용한다. (map, filter, reduce 등 )
4. 선언형 코드로 바꾸는 연습을 한다.

어려워도 조금씩 이해하려고 해보자, 조금 더 좋은 코드가 나올것이다!.
