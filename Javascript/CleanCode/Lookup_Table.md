# Lookup Table

Lookup Table은 친숙하지 못한 용어이다. 강의를 들어서 그렇지 듣기전까지는 처음들어보는 용어였다.  
Lookup Table은 배열이나 연관배열로 된 데이터 구조로 쉽게 생각하면 key와 value로 된 json과 비슷한 형태의 테이블이라고 생각하면 된다.  
엑셀을 자주 사용했다면 LOOKUP, VLOOKUP 등을 생각하면 된다.
이전 문서에서 if , elseif, else등을 사용하지 말고 길어질 경우 switch문을 사용하자라는 글을 작성하였는데, 로직자체는 별로다를 것이 없지만 switch가 조금 더 케이스가 눈에 띄고 이해하기 쉽게 떄문에 사용하자라고 했다, 하지만 실제로 분기가 많아질 경우 switch문도 길어져서 보기 힘든건 마찬가지이다. 요구사항이 많아 질수록 코드도 길어진다는 것이다. 이럴 때 Lookup Table을 사용하는 것이좋다.  
Lookup Table이란 용어는 자바스크립트의 표준 용어가 아니니 다른 곳에 유래가 있다 생각하면 되고, 아래 코드를 보자 .

```
function getUserType(type) {
    switch (key) {
        case 'ADMIN' :
            return '관리자';
        case 'INSTRUCTOR' :
            return '강사';
        case 'STUDENT' :
            return '수강생';
        default :
            return '해당없음';
    }
}
```

위 코드는 switch문으로 작성된 코드이고 , Lookup Table을 활용하면 아래와 같이 코드를 작성할 수 있다.

```
function getUserType(type) {
    const USER_TYPE = {
        ADMIN : '관리자',
        INSTRUCTOR : '강사',
        STUDENT: '수강생',
    };

    return USER_TYPE[type] || '해당없음';
}
```

switch문과 아래 코드를 비교해보면 분기문이 객체의 key와 value로 이루어져 있는 것을 알 수 있다. USER_TYPE는 누가봐도 변하면 안된다는 값으로 대문자로 상수로 표현하고 있고 type이 매개변수로 들어오면 USER_TYPE에 대괄호[]를 사용하여 그 type값을 넘겨주고 있는지 없는지 확인하는 것이된다. 결국 USER_TYPE에 .닷 연산자로 하나하나 접근해서 있는지 찾아보는 것이다. 있으면 그에 대한 값을 리턴하고 없으면 해당없음이 리턴될 것이다.
이렇게 관리를 하게 된다면 다른 요구사항이 생겨도 USER_TYPE의 값만 하나씩 생성하면 되니 switch 문 if문 보다 훨씬 더 보기 쉽고 간결하게 확인이 가능할 것이다.
