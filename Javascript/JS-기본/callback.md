# 콜백함수

어떤 다른 함수에 매개변수로 함수를 넘겨준 것을 의미한다.
즉 함수의 매개변수에 함수를 넘겨준다 라는 것이다.

```
function checkMood(mood) {
  if (mood === "good") {
    // 기분 좋을 때 동작
    sing();
  } else {
    // 기분 안 좋을 때 동작
    cry();
  }
}

function cry() {
  console.log("ACTION :: CRY");
}

function sing() {
  console.log("ACTION :: SING");
}

function dance() {
  console.log("ACTION :: DACNE");
}

checkMood("good");
checkMood("sad");

결과 :
        ACTION :: SING
        ACTION :: CRY
```

위 함수를 보면 기분을 체크하여 그 기분에 해당하는 동작을 수행하는 함수이다.
기분이 좋으면 노래를하고 (sing 함수 호출), 그렇지 않으면 우는 것이다(cry 함수 호출).
하지만 dance함수는 선언만 되어있고 사용하지 않고 있다. 내가 원하는 것은 기분에 따라서 세가지 동작을 유기적으로 하는 것인데 고정된 행동 밖에는 못하는 함수가 되어 버렸다. 그래서 조금 더 유기적으로 행동 하기 위해서 CallBack함수라는 것을 사용한다.

```
function checkMood(mood, goodCallback, badCallback) {
  if (mood === "good") {
    // 기분 좋을 때 동작
    goodCallback();
  } else {
    // 기분 안 좋을 때 동작
    badCallback();
  }
}

function cry() {
  console.log("ACTION :: CRY");
}

function sing() {
  console.log("ACTION :: SING");
}

function dance() {
  console.log("ACTION :: DACNE");
}

checkMood("sad", sing, cry);
checkMood("good", dance, sing);

결과 :
        ACTION :: CRY
        ACTION :: DACNE
```

위 함수를 보면 일단 checkMood() 함수에서 매개변수로 mood, goodCallback, badCallback을 받고있다.
차례대로 기분, 기분좋을 때 함수, 기분나쁠때 함수를 매개변수로 받는것이다.
호출문을 보면 checkMood("sad", sing, cry); 함수를 호출하면서 인자로 sad라는 기분과 기분좋을 때 할 sing이라는 함수명 , 기분나쁠 때 할 cry이라는 함수를 넘겨주고 있다.
즉 goodCallback 은 sing함수를 받고 badCallback은 cry함수를 가지게 된다.
그 후 기분이 sad이니 else문으로 들어가서 badCallback()함수가 실행 되는데 badCallback은 cry() 함수이다. 그래서 출력이 ACTION :: CRY가 된다.

다시 두번째 함수호출로 살펴보자.
checkMood("good", dance, sing);
checkMood() 함수를 호출하면서 넘겨주는 값은 **_ "good" 이라는 기분의 문자열 / dance함수 / sing함수 _**
checkMood() 함수가 받는 값은 **_ mood에 "good" / goodCallback에 dance()함수 / badCallback에 sing()함수 _**
그 후 mood === "good" 임으로 if (mood === "good") { goodCallback(); }가 실행되며 goodCallback()는 dance()함수 임으로 console.log("ACTION :: DACNE");가 실행되어
결과는 ACTION :: DACNE 이 나오게 된다.

이렇게 함수의 파라매터로 함수를 넘기는 것이 콜백함수이다. 콜백함수는 배열 내장 함수에서 더 자세하게 다룰 예정이다.
