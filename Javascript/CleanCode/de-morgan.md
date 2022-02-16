# 드모르간의 법칙

아주 간단한 함수 하나를 보자.

```
const isValidUser = true;
const isValidToken = true;

if (isValidUser && isValidToken) {
    console.log('로그인 성공!);
}
```

이 함수는 논리값 2개를 AND 연산하여 모두 true일 경우 로그인 성공을 출력하고 있다. 하지만 로그인실패가 추가 되어야 한다면 어떻게 될까 ?
실제로 isValidUser, isValidToken값은 서버에서 받아온 값이라 가정하면 개발자는 가지고 있는 두 값을 변경시켜서 로그인실패를 구현할 수 밖에없다. 그럼 아래와 같은 코드를 짤 수 있다.

```
const isValidUser = false;
const isValidToken = false;

if (!(isValidUser && isValidToken)) {
    console.log('로그인 실패!);
}
```

잘 해결됬다고 생각했는데 다른 로직들이 계속 추가 된다면 어떨까 ? 개발자들이 고통스러워 질것이다. 이떄 드모르간의 법칙을 활용하면 된다.  
아주 간단하게 위 코드는 반대로 OR연산으로 돌려버리면 !isValidUser || !isValidToken 과 같다. 둘다 false일때 false이니까! 이것을 활용해서 아래와 같이 부정연산만 제거해버리면 된다.

```
const isValidUser = false;
const isValidToken = false;

if (isValidUser || isValidToken) {
    console.log('로그인 실패!);
}
```

2개의 값이 모두 false이기 때문에 로직도 정상적이며 코드는 더 보기 좋아 졌다.  
이 처럼 드모르간의 법칙은 true is not true, false is not false가 핵심이다. 적절한 상황에서 연산이 길이질 때 사용하면 아주 깔끔한 코드를 작성할 수 있다.

[드 모르간의 법칙](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=asd7979&logNo=30106982953)
