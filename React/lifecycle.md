# React Lifecycle 제어하기 - useEffect

Lifecycle은 한국말로 하면 생애주기라고 한다. 생애주기란 일반적으로 시간에 흐름에 따라 탄생부터 죽음에 이르는 순간에 따른 단계적인 과정이라고 하는데,  
0~5세가 영유아 6~12세 아동기, 13~18세 청소년기, 19~29세 청년 30~64세 성인기, 65~ 노년기라고 하는 이러한 과정이다.

이런 인간말고도 소프트웨어에서도 생애주기라는 Lifecycle이란 단어가 많이 사용되는데, 어떤 프로그램이 실행되고 종료되는 과정을 나타내기 위해 이러한 단어를 사용하게 된다.

React 컴포넌트도 이런 생명주기 Lifecycle 을 가지게 되는데 그렇다고 애기 React에서 어른 React 노인 React가 되는 것이 아니라, 나름 비슷한 면이 있다.  
말이 어렵지만 쉽게 풀이하자면 React 컴포넌트의 생애주기는 탄생 -> 변화 -> 죽음에 빗대어 얘기해보자.

1. 탄생이란 것은 화면에 나타나는 것이고 이 부분을 Mount 마운트라고 부른다.
2. 변화라는 것은 업데이트(리렌더)가 되는 순간이고 이 부분을 Update 업데이트라고 부른다.
3. 죽음은 화면에서 사라지는 것이고 이 죽음을 UnMount 언마운트라고 부른다.

그럼 React Lifecycle 제어하기라는 것은 우리에게 어떤 의미가 있을까 ? 제어한다는 것은 엄청 어렵고 고난도의 기술을 요하는 것이 아니라,  
그냥 컴포넌트가 탄생하고 변화하고 죽음에 이를 때 우리가 각각 어떤 작업을 수행시킬 수 있다는 것을 Lifecycle을 제어한다 또는 이용한다 라고 한다.

예를들어 탄생 때에는 초기화작업을 시킨다던가, 변화가 일어날 때 예외 처리 작업을 해준다던가, 또는 사라질 때 해당 컴포넌트가 사용한 메모리를 반환하게 하는 것들이다.

자 그럼 사용해보자, React는 기본적으로 Lifecycle마다 사용할 수 있는 메서드가 존재한다.

1. ComponentDidMount : Mount 될 때 사용 할 수 있는 함수
2. ComponentDidUpdate : Update 될 때 사용 할 수 있는 함수
3. ComponentWillUnmount : UnMount 될 때 사용 할 수 있는 함수

하지만 아쉽게도 이런 메서드는 class형 컴포넌트에서만 사용할 수 있다. 하지만 지금까지 컴포넌트를 화살표 함수를 통해서 함수형으로 제작하였는데, 함수형태로 제작하는 것을 함수형 컴포넌트라고 한다. 지금까지 class형 컴포넌트를 사용하지 않아서 이런 메서드들을 사용할 수가 없었다. 근데 정말 근본적으로 함수형컴포넌트는 이러한 Lifecycle을 제어하는 메서드 말고도 상태를 관리하는 State도 사용할 수 없다. State라는 기능도 class형 컴포넌트만 이용할 수 있는 기능이기 때문이다.  
그런데 useState를 사용하여 잘 사용해왔는데, 이렇게 use 키워드를 붙여서 원래 class형 컴포넌트가 가진 기능을 함수형 컴포넌트에서 Hooks(후킹, 낚아채다)해서 사용할 수 있는 기능을 React Hooks라고 한다. 이 Hooks가 개발되어서 use키워드를 붙여서 함수처럼 불러와서 사용할 수 있게 된것이다.  
대표적으로 useState, useEffect, useRef 등 이 있다.  
useEffect는 Lifecycle을 제어하는 메서드들을 낚아올 수 있는 Hooks라고 생각하면 된다.

그러면 근본적으로 왜 모든 기능을 사용할 수 있는 class형 컴포넌트가 아니라 Hooks라는 기능까지 사용해 가면서 함수형 컴포넌트로 쓰는 걸까 ?  
React Hooks는 2019년 06월에 정식 출시된 기능이다. 그래서 2019년 이전에 React를 배운사람이라면 대부분 class형 컴포넌트를 사용하였다.  
하지만 이 class형 컴포넌트에는 고질적인 문제가 하나있다. 바로 함수형 컴포넌트보다 같은 기능을 제작하는데 굉장히 많은 코드를 작성해야 하고 복잡하며, 가장 치명적인 단점은 중복코드를 많이써야하는 단점이 있다.  
그래서 최신 실제 제품을 개발하고 운영하는 회사도 class형을 배제하고 함수형으로 제작/사용하려는 추세이다.

자 그럼 useEffect를 어떻게 사용하는지 알아보자.

```
import React, { useEffect } from "react";

useEffect(() => {
    // todo
}, []);
```

위 방법이 useEffect를 사용하는 방법이다. 2개의 파라미터를 전달하게 되는데, 첫번째 파라미터는 콜백함수 두번째 파라미터는 뎁스라고 하여 Dependency Array(의존성 배열)을 전달한다. 이 두번째 파라미터인 뎁스에 값이 하나라도 변화하면 첫번째 파라미터인 콜백함수가 수행된다. 그러니까 두번째 파라미터인 뎁스배열에 변화하는 값을 넣어두면, 콜백함수가 값이 변화할 때 마다 수행되는 것이다.

아래코드를 자세히 살펴보면 useEffect를 어떻게 사용해야 하는지 알 수 있다.

```
import React, { useEffect, useState } from 'react';

const Lifecycle = () => {
    const [count, setCount] = useState(0);
    const [text, setText] = useState('');

    const [isVisible, setisVisible] = useState(false);
    const toggle = () => setisVisible(!isVisible);

    // 탄생
    useEffect(() => {
        console.log('Mount!');
    }, []);

    // 업데이트

    // 전체 감지
    // useEffect(() => {
    //     console.log('Update!');
    // });

    // count만 감지
    useEffect(() => {
        console.log(`count is update : ${count}`);
        if (count > 5) {
            alert('Count Over!!!');
            setCount(1);
        }
    }, [count]);

    // text만 감지
    useEffect(() => {
        console.log(`text is update : ${text}`);
    }, [text]);

    // 죽음
    const UnmountTest = () => {
        useEffect(() => {
            console.log('Mount!');

            // unMount 시점에 실햄하게됨
            return () => {
                console.log('Unmount!');
            };
        }, []);

        return <div>Unmount Testing Component</div>;
    };

    return (
        <div style={{ padding: 20 }}>
            <div>
                {count}
                <button
                    onClick={() => {
                        setCount(count + 1);
                    }}
                >
                    +
                </button>
            </div>
            <div>
                <input value={text} onChange={(e) => setText(e.target.value)} />
            </div>
            <div>
                <button onClick={toggle}>ON/OFF</button>
                {isVisible && <UnmountTest />}
            </div>
        </div>
    );
};

export default Lifecycle;
```
