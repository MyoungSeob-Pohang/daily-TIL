# Early-Return

Early-Return이란 무엇일까 ? Early는 빠른, 일찍이란 뜻인데 얼마나 빨리 리턴하길래 Early리턴이라는 이름이 붙었을까 ?

```
function loginService(isLogin, user) {
    if(!isLogin) {
        if(checkToken()) {
            if(!user.nickName) {
                return registerUser(user);
            } else {
                refreshToken();

                return '로그인 성공';
            }
        } else {
            throw new Error('No Token');
        }
    }
}
```

위 코드는 로그인 관련된 함수이다. 만약 로그인이 안되어 있다 라는 요청이 서버에서 날아오면 토큰이 있는지 확인하고 유저의 닉네임이 존재하지 않으면 가입한적이 없는 유저이니 바로 registerUser() 회원가입으로 보내버리고 다 있는데 닉네임이 존재하면 refreshToken()을 동작시키고 로그인 성공을 반환한다.

1. 로그인여부 확인
2. 토근유무 확인
3. 원래 가입된 유저인지 확인
4. 가입된 유저면 로그인 아니면 회원가입

딱 봐도 로직이 복잡하고 코드만 봐도 복잡하다. 하나하나 줄여보자.

일단 로그인 유무를 확인하는 if를 따로 때내어서 분리하였다. 로그인이 되어 있으면 바로 return해서 종료시켜버리면 된다. 모든 로직이 로그인이 안되어 있을 때 실행되는 로직인데 로그인이 되어있다면 필요가 없기 때문이다 .

```
if(isLogin){
    return;
}

function loginService(isLogin, user) {
    if(checkToken()) {
        if(!user.nickName) {
            return registerUser(user);
        } else {
            refreshToken();

            return '로그인 성공';
        }
    } else {
        throw new Error('No Token');
    }
}
```

위 처럼 때어내서 일찍 종료시키는 것을 Early-Return 이다. 미리 종료시키면서 사람이 생각하기도 편하다. 한번더 줄여보자.  
이번에는 토근이 없을 때도 로직이 더 실행될 필요가 없기 때문에 이 부분도 Early-Return 해보자.

```
function loginService(isLogin, user) {
    if(isLogin){
        return;
    }

    if(!checkToken()) {
        throw new Error('No Token');
    }

    if(!user.nickName) {
        return registerUser(user);
    } else {
        refreshToken();

        return '로그인 성공';
    }
}
```

이번엔 토큰이 없으면 바로 Error메세지를 보낼 수 있도록 해버렸다. 훨씬 가독성이 좋아졌다. 그럼 나머지 남아있는 if else문도 줄일 수 있지 않을까 ?

```
function loginService(isLogin, user) {
    if(isLogin){
        return;
    }

    if(!checkToken()) {
        throw new Error('No Token');
    }

    if(!user.nickName) {
        return registerUser(user);
    }

    refreshToken();

    return '로그인 성공';
}
```

딱 봐도 사람이 생각하기 정말 좋은 코드이다. 순서대로 읽으면 된다.

1. 로그인이 되있으면 종료되는 구나
2. 토큰이 없으면 에러가 나는구나
3. 닉네임이 없으면 회원가입으로 가는구나
4. 다 아니면 토큰을 새로고침하고 로그인 성공하는구나

어떤가 더 알아보기 쉽게 인간이 읽기 편하게 만들었다. 하지만 로직은 변한게 없다. 이런게 바로 좋은 코드 아닐까 ? 누가봐도 이해하기 좋고 명확한 코드.  
이렇게 되면 함수를 더 유동적으로 쪼개거나 분리하기도 편하다.
