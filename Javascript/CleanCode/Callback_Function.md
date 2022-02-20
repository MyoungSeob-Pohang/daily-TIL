# Callback Function

자바스크립트에서 콜백함수라고 하면 여러가지 설명들이 있지만, 함수의 실행권을 다른 함수에게 위임하는 것이라고 생각 할 수도있다. 보통 콜백함수를 많이 사용하고 있음에도 콜백함수를 사용하지 않고 있다고 생각하는 경우도 많은데 addEventListener를 사용할 때에도 우리는 콜백함수를 사용하고 있다.

```
someElement.addEventListener('click', Callback Function);
```

어떤 엘리먼트에 메서드로 addEventListener API를 호출하면 클릭이벤트가 감지되었을 때 뒤에 Callback Function이 실행된다. 그럼 뒤에 있는 Callback Function는 개발자의 제어권이 addEventListener에게, 클라이언트에게 넘어간 것이다. 예제로 알아보자.

```
function register() {
    const inConfirm = confirm('회원가입에 성공했습니다.');

    if (inConfirm) {
        redirectUserInfoPage();
    }
}

function login() {
    const inConfirm = confirm('로그인에 성공했습니다.');

    if (inConfirm) {
        redirectIndexPage();
    }
}
```

회원가입과 로그인 예제가 있다. confirm를 호출하고 confirm이 true이면 redirectUserInfoPage, redirectIndexPage로 각각 보내고 있다. 이럴 때 콜백함수를 생각할 수 있는데, 사실 굉장히 간단하다.

```
function confirmModal(message, cbFunc) {
    const inConfirm = confirm(message);

    if(inConfirm && cbFunc) {
        cbFunc();
    }
}

function register() {
    confirmModal('회원가입에 성공했습니다.', redirectUserInfoPage)
}

function login() {
    confirmModal('로그인에 성공했습니다.', redirectIndexPage)
}

```

콜백함수로 나누어진 함수를 하나로 통합했다, confirmModal을 호출하면서 메세지와 콜백함수를 넘겨주고 confirmModal에서는 메세지가 참이고 콜백함수가 있으면 콜백함수를 실행한다. 가끔 () 을 넣어서 함수를 실행시키는 사람도 있는데, 콜백함수는 콜백을 받아서 실행하는것이기 때문에 함수 그 자체로 넘겨주어야 한다.  
이렇게 해서 콜백함수를 통해서 제어권을 다른 함수에 넘기고 그 함수에서 만든 함수를 제어하고 실행시키는 권한을 가지게 된다. 콜백함수를 콜백지옥, 안좋은 패턴이라고 생각하지말고 제어권을 위임할 수 있다는 것을 알면 더 클린코드를 작성할 수 있을 것이다.
