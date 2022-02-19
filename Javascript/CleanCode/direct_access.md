# 직접 접근 지양하기

이제 순수 자바스크립트를 넘어서 다양한 상태를 다루다보면 객체를 읽고, 쓰고 할 일이 많아진다.

```
const model = {
    isLogin: false,
    isValidToken: false,
}

function login() {
    model.isLogin = true;
    model.isValidToken = true;
}

function logout() {
    model.isLogin = false;
    model.isValidToken = false;
}

someElement.addEventListener('click', login);
```

위 코드를 보면 로그인, 로그아웃을 할 수 있는 함수가 있고 특정 element 를 클릭했을 때 이벤트 리스터를 호출하여 login함수가 실행 되는 코드도 있다.  
전혀문제없는 코드같지만 문제점이 하나있다, 바로 함수들이 model에 직접접근해서 값을 변경하는 것이다. 너무 쉽게 접근해서 값을 변경하는데, 실제로 model을 렌더하는 곳, 혹은 어딘가에 model이 그려지는 곳이 있는데 그만큼 model이 중요한데 너무 열려있다는 것이다. 그러면 어떻게 하느냐 ? 따로 접근하는 함수를 빼주는 것이다.

```
const model = {
    isLogin: false,
    isValidToken: false,
}

function setLogin(bool) {
    model.isLogin = bool;
}

function setValidToken(bool) {
    model.isValidToken = bool;
}

function login() {
    setLogin(true);
    setValidToken(true);
}

function logout() {
    setLogin(false);
    setValidToken(false);
}

someElement.addEventListener('click', login);
```

위 코드를 보면 setLogin, setValidToken라는 함수를 따로 만들어서 model에 접근을 하고있고, login, logout에서는 setLogin, setValidToken에 접근을 하고있다.  
어디서나 model에 접근해서 사용하는 방법을 쓰지않고 함수에 위임해서 추상화를 시킨것이다. 이런식으로만 사용해도 코드를 안전하게 변화시킬 수 있고 model이 어디서 어떻게 변하는지 알 수 있기 때문에 디버깅도 쉬워진다.  
데이터에 접근해야 할때는 안전하게 접근해야한다. redux나 Vuex같은 flex패턴을 작성할 때에도 상태를 변화시키려면 action을 거치고 reducer를 거쳐야 상태가 변한다. 왜 이렇게 까지 상태 변화하는게 힘들어야 할까? 그 이유는 바로 예측 가능한 상태를 만들기 위해서이다.
자바스크립트로 코드를 짤 때 우리도 예측 가능한 코드를 작성해서 동작이 예측 가능한 앱을 만드는 것이 굉장히 중요하다. 그래서 model에는 직접 접근을 하지 않고 model에 대신 접근해서 사용하는 함수롤 사용하고 외부에서는 모델에 직접접근이 불가능하게 코드를 작성하는 것이다. 자바스크립트 접근자, 설정자인 get, set도 이에 해당한다.
다른 언어의 getter, setter도 마찬가지이고 안전하게 접근해서 데이터를 조작하는 것이 아주 중요하다.
