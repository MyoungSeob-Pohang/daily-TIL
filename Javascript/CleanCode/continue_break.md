# Continue & Break

자바스크립트의 배열메서드에 익숙해지면 많이하는 실수 중에 Continue & Break 도 있다. Continue & Break는 특정 레이블이나 문의 흐름을 제어하는데, Continue는 그 흐름을 제어하여 반복이나 행위를 돌릴 수 있고, Break는 흐름을 끊고 다음으로 넘어갈 수 있다.  
그러다 보니 for문으로 코드를 작성하는게 익숙했던 사람들이 주로 코드를 짜다가 break나 Continue를 붙여서 써왔을 텐데 forEach를 쓰다보면 break나 continue가 되지 않아 당황하게 된다.

```
const orders = ['first', 'second', 'third'];

orders.forEach((order)=>{
    if(order === 'second'){
        break;
    }

    console.log(order);
})
```

위 코드를 보자, 개발자가 의도한 방향은 second를 만나면 종료하고 third가 출력되지 않기를 바랄 것이다. 하지만 결과는 신텍스에러(SyntaxError)가 나타난다. forEach가 아니라 map filer 어떤 것을 써도 동일하다. 그럼 어떻게 해야할까 ? for-of문 , for-in문, for문을 쓰거나 try catch문을 사용하는 것이 방법중에 하나이다.  
또 다른 방법으로는 every, some, find, findIndex같이 특정 조건이 되면 반복이 종료되는 배열메서드를 조합하여 사용하여도 된다.
