# 최적화 2 - 컴포넌트 재사용 / React.memo

컴포넌트 재사용이란 어떤 말일까, 예시를 통해 알아보자.  
예를들어 App컴포넌트의 자식컴포넌트로 CountView컴포넌트와 TextView컴포넌트가 존재한다고 하자, 부모인 App에서 state로 count, text를 가지고 있고  
CountView컴포넌트에게 prop로 count를 TextView컴포넌트에게 prop로 text를 내려준다고 하자. 자 그런데 코드가 setCount(10); 이다. 그럼 count의 값이 바뀌게 됨으로 App컴포넌트가 리렌더가 되고 자동적으로 자식컴포넌트인 CountView, TextView도 리렌더가 된다. 하지만 너무 비효율적인거 같다. count의 값만 바꿧으니 CountView컴포넌트만 리렌더가 되면 되는데 왜 상관없는 TextView도 리렌더가 되어야할까 ? 너무 낭비가 심한거 같고 성능에 문제가 있을 거 같다.  
그럼 자식컴포넌트에게 조건을 주면 될거같다. 각각 prop로 전달되는 값이 변경될 때만 자기자신이 변경되면 어떨까? count가 변경되면 CountView만, text가 변경되면 TextView만 변경되게 하면된다. 이럴때 사용하는 것이 바로 React.memo이다. React.memo는 쉽게 얘기하면 컴포넌트에게 업데이트 조건을 걸어버리는 것이다.

ReactDoc를 보게 되면 React.memo를 이렇게 설명한다.  
React.memo는 고차 컴포넌트이다. 그럼 고차컴포넌트라는 것은 무엇일까 ? 고차컴포넌트는 컴포넌트를 가져와 새 컴포넌트를 반환하는 함수이다.

```
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```

뭔가 함수를 호출하는데 매개변수로 컴포넌트를 전달했더니 더 진화된 컴포넌트를 반환하는 그런 친구구나 라고 생각하면 된다.

React.memo의 사용법은 아래와 같다.

```
const MyComponent = React.memo(function MyComponent(props) {
    <!-- props를 사용하여 렌더링 -->
});
```

**React.memo는 부모의 의한 리렌더만 제어하지 자기자신의 값이 바뀌면 당연히 리렌더가 된다.**

그럼 사용방법을 알았으니 실제 코드로 알아보자. 아래 코드는 카운터를 올리는 + 버튼과 텍스트를 바꾸는 input으로 구성되어 있고 이 값들을 state로 관리하는데 자식 컴포넌트에게 그 값을 prop으로 전달하고 있다. 그런데 자식 컴포넌트는 React.memo를 사용한 업그레이드 된 컴포넌트로 자기 자신의 값이 바뀌지 않으면 리렌더가 되지 않는다.

```
import React, { useEffect, useState } from 'react';

const TextView = React.memo(({ text }) => {
    useEffect(() => {
        console.log(`Update :: text : ${text}`);
    });

    return <div>{text}</div>;
});

const CountView = React.memo(({ count }) => {
    useEffect(() => {
        console.log(`Update :: count : ${count}`);
    });

    return <div>{count}</div>;
});

const OptimizeTest = () => {
    const [count, setCount] = useState(1);
    const [text, setText] = useState('');

    return (
        <div style={{ padding: 50 }}>
            <div>
                <h2>count</h2>
                <CountView count={count} />
                <button
                    onClick={() => {
                        setCount(count + 1);
                    }}
                >
                    +
                </button>
            </div>
            <div>
                <h2>text</h2>
                <TextView text={text} />
                <input value={text} onChange={(e) => setText(e.target.value)} />
            </div>
        </div>
    );
};
export default OptimizeTest;
```

```
const TextView = React.memo(({ text }) => {
    useEffect(() => {
        console.log(`Update :: text : ${text}`);
    });

    return <div>{text}</div>;
});

const CountView = React.memo(({ count }) => {
    useEffect(() => {
        console.log(`Update :: count : ${count}`);
    });

    return <div>{count}</div>;
});
```

이 코드를 자세하게 보자. TextView 와 CountView 둘다 prop을 받아서 div에 담아 리턴하고 있는데 React.memo를 사용하였다. 그리고 그 결과를 자세히 알아보기 위해 useEffect로 렌더링이 될때 마다 값을 찍어보게 하였다. 이렇게 되면 실제로 어떻게 되느냐 ? text가 변경될 때는 TextView만 리렌더 되고 CountView를 리렌더 되지 않는다.
마찬가지로 count가 변경될 때는 CountView는 리렌더 되지만 TextView는 리렌더 되지않는다. 그 결과를 console에 찍어보면 더 자세히 알 수 있다. 이렇게 되면 연산의 중복을 막고 불필요한 리렌더를 막아서 성능에 최적화를 할 수 있게된다. 하지만 여기서 궁금한 점이 있다. 만약 state를 변화시킬 때 현재 가진 값을 그대로 준다면 리렌더가 될까 ?

```
const CounterA = React.memo(({ count }) => {
  useEffect(() => {
    console.log(`CountA Update - count : ${count}`);
  });
  return <div>{count}</div>;
});

const CounterB = React.memo(({ obj }) => {
  useEffect(() => {
    console.log(`CountB Update - count : ${obj.count}`);
  });
  return <div>{obj.count}</div>;
});

const OptimizeTest = () => {
  const [count, setCount] = useState(1);
  const [obj, setObj] = useState({ count: 1 });

  return (
    <div style={{ padding: 50 }}>
      <div>
        <h2>Counter A</h2>
        <CounterA count={count} />
        <button onClick={() => setCount(count)}>A Button</button>
      </div>
      <div>
        <h2>Counter B</h2>
        <CounterB obj={obj} />
        <button onClick={() => setObj({ count: 1 })}>B Button</button>
      </div>
    </div>
  );
};
```

CounterA는 count로 state를 관리하고 CounterB는 객체인 obj로 state를 관리하고 있다. 근데 onClick이벤트를 보면 같은 값(setCount(count) / setObj({ count: 1 }))을 계속 전달해 주고있는 것을 알 수 있다.  
그리고 CounterA ,CounterB 모두 React.memo를 사용하고 있다.

이때 콘솔로 결과를 보게 되면 CounterA는 리렌더가 일어나지 않는다. 같은 값을 전달해주기 때문이다. 하지만 CounterB는 같은 값을 전달해도 계속 리렌더가 된다. 왜 그럴까 ?
CounterB는 객체이고 자바스크립트는 객체를 비교할 때 **_얕은비교_**를 하기 때문이다.
그럼 얕은 비교란 또 무엇일까 ?  
얕은비교는 객체의 주소에 의한 비교이다 , 예를 들면

```
a = {count: 1};
b = {count: 1};
```

이러한 값이 존재한다면 선언 후 메모리주소에 할당이 될 것이다.  
자 예를 들어 a는 10번지, b는 13번지에 저장이 되었다. 값은 같지만 주소가 다르다.  
그럼 자바스크립트에서 이런 얕은비교를 한다면 값을 비교하는게 아니라 같은 주소에 있는지를 비교한다. 그래서 a === b결과는 다르다 라고 나온다.  
사람에게 비유를 하자면 홍길동은 나이 20살에 체중 70키로 키 175이고 아무개도 나이 20, 체중 70, 키 175이다. 하지만 아무개랑 홍길동은 같은 사람이 아니다, 즉 다르다. 객체도 이와 똑같이 얕은 비교를 하는 것이다. 하지만 코드를 아래와 같이 작성하면 상황이 달라진다.

```
a = {count: 1};
b = a;
```

이렇게 되면 a는 count 1값을 가지고 10번지 주소에 할당이되는데 b는 a와 같다라는 식을 통해 b도 count 1값을 가진 10번지를 가지게 된다.

그럼 이 CounterB는 계속 렌더링이 되어야할까 ? 다른 방법이 있다. doc문서를 보게 되면 areEqual이라는 함수의 설명이있다. 다른 비교의 동작을 원한다면 React.memo에게 두번째 인자로 별도의 비교함수를 제공하면 된다 라고 정의되어있다.
그럼 아래코드와 같이 변경할 수 있다.

```
import React, { useEffect, useState } from "react";

const CounterA = React.memo(({ count }) => {
  useEffect(() => {
    console.log(`CountA Update - count : ${count}`);
  });
  return <div>{count}</div>;
});

const CounterB = ({ obj }) => {
  useEffect(() => {
    console.log(`CountB Update - count : ${obj.count}`);
  });
  return <div>{obj.count}</div>;
};

const areEqual = (prevProps, nextProps) => {
  if (prevProps.obj.count === nextProps.obj.count) {
    return true;
  }
  return false;
};

const MemoizedCounterB = React.memo(CounterB, areEqual);

const OptimizeTest = () => {
  const [count, setCount] = useState(1);
  const [obj, setObj] = useState({
    count: 1
  });

  return (
    <div style={{ padding: 50 }}>
      <div>
        <h2>Counter A</h2>
        <CounterA count={count} />
        <button onClick={() => setCount(count)}>A Button</button>
      </div>
      <div>
        <h2>Counter B</h2>
        <MemoizedCounterB obj={obj} />
        <button onClick={() => setObj({ count: 1 })}>B Button</button>
      </div>
    </div>
  );
};

export default OptimizeTest;
```

CounterB에 React.memo를 빼고 areEqual함수를 만들어서 prevProps, nextProps두개의 인자를 받게되는데 여기서 return true가 되면 두 props가 같은 것이어서 리렌더가 일어나지 않게 되고 false가 되면 두 props가 다른것이니 리렌더가 된다. prevProps.obj.count와 nextProps.obj.count가 서로 같은지 확인하고 그 결과를 return 하고 있다.
그리고 const MemoizedCounterB = React.memo(CounterB, areEqual); MemoizedCounterB에 CounterB와 areEqual를 사용한 memo값인 새로운 컴포넌트를 담고 그것을 출력해보았다.

```
<MemoizedCounterB obj={obj} />
```

이렇게 되면 같은 객체를 전달하여도 리렌더가 되지 않는다.
