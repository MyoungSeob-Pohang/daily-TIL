# 조건문

조건문은 영어로 ‘만약(~한다면)’이라는 뜻의 ‘if’처럼 가정이나 조건을 내거는 문장이다.
어떤 연산의 참 거짓에 따라서 다른 명령을 각각 실행할 수 있게 한다.

## if , if-else

```
let a = 3;

if(a <= 5){
    console.log("a는 5보다 작거나 같습니다.");
}else{
    console.log("a는 5보다 큽니다.");
}

// 결과 : a는 5보다 작거나 같습니다.
```

else는 if 조건에 일치하지 않으면 실행한다. 만약 if와 else 사이에 조건을 더 추가하고 싶으면 else if라는 키워드를 사용하여 계속 이어 붙일 수 있다.
결국 마지막 else는 앞의 if , else if조건이 모두 참이 아닐경우 실행된다.

```
let a = 4;

if(a <= 3){
    console.log("a는 3보다 작거나 같습니다.");
}else if(a === 4){
    console.log("a는 4입니다.")
}else{
    console.log("a는 5이상 입니다.");
}

// 결과 : a는 4입니다.
```

## switch문

switch문을 사용한 비교법은 특정 변수를 다양한 상황에서 비교할 수 있게 해줍니다. 코드 자체가 비교 상황을 잘 설명한다는 장점도 있다.
default문도 있지만 필수는 아니며 break문은 조건문을 빠져나가는 코드입니다.
만약 break문이 존재하지 않으면 해당코드 break를 만날때 까지 모든 코드를 출력합니다.

```
let 국적 = "한국";

switch(국적){
    case "일본":
        console.log("일본");
        break;
    case "영국":
        console.log("영국");
        break;
    case "중국":
        console.log("중국");
        break;
    case "홍콩":
        console.log("홍콩");
        break;
    case "한국":
        console.log("한국");
        break;
    default :
        console.log("미분류");
        break;
}

// 결과 : 한국
```

```
let 국적 = "일본";

switch (국적) {
  case "일본":
    console.log("일본");
  case "영국":
    console.log("영국");
  case "중국":
    console.log("중국");
  case "홍콩":
    console.log("홍콩");
  case "한국":
    console.log("한국");
  default:
    console.log("미분류");
}

결과:   일본
        영국
        중국
        홍콩
        한국
        미분류
```
