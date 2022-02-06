# useReducer 복잡한 상태 관리 로직 분리하기

현재 App컴포넌트에는 복잡하고 많은 상태변화 처리 함수들이 있다. onCreate, onEdit, onRemove가 있고 굉장히 코드가 길다.  
저 세함수 모두 data의 값을 가져다 쓰고있기 때문에 함수 밖에서는 만들 수 없었기 때문이다. 하지만 컴포넌트 밖에 위치하게 만드는 기능이있다.
바로 useReducer이다. 이것을 활용하면 상태변화로직을 컴포넌트에서 분리시킬 수 있다.

예를 들어 아래와 같은 코드가 있다고 하자.

```
return (
    <div>
        {count}
        <button onClick={add1}>add 1</button>
        <button onClick={add10}>add 10</button>
        <button onClick={add100}>add 100</button>
        <button onClick={add1000}>add 1000</button>
        <button onClick={add1000}>add 10000</button>
    </div>
);

const Count = () => {
    const [count, setCount] = useState(1);

    const add1 = () => {
        setCount(count + 1);
    };
    const add10 = () => {
        setCount(count + 10);
    };
    const add100 = () => {
        setCount(count + 100);
    };
    const add1000 = () => {
        setCount(count + 1000);
    };
    const add10000 = () => {
        setCount(count + 10000);
    };
}
```

위 코드를 보면 상태변화 함수 5개를 각각 count컴포넌트 안에 작성을 해야했다, 이렇게 하면 코드가 길어지고 복잡해질 수 밖에없다. 하지만 useReducer를 쓴다면 아래와 같이할 수 있다.

```
const Counter = () => {
    const [count, dispatch] = useReducer(reducer, 1);

    return (
        <div>
            {count}
            <button onClick={() => dispatch({ type: 1 })}>add 1</button>
            <button onClick={() => dispatch({ type: 10 })}>add 10</button>
            <button onClick={() => dispatch({ type: 100 })}>add 100</button>
            <button onClick={() => dispatch({ type: 1000 })}>add 1000</button>
            <button onClick={() => dispatch({ type: 10000 })}>add 10000</button>
        </div>
    );
};

const reducer = (state, action) => {
    switch (action.type) {
        case 1:
            return state + 1;
        case 10:
            return state + 10;
        case 100:
            return state + 100;
        case 1000:
            return state + 1000;
        case 10000:
            return state + 10000;
        default:
            return state;
    }
}
```

reducer라는 상태변화 함수를 컴포넌트 밖에 분리를 하여 switch 문법처럼 쉽게 처리할 수 있다.

```
const [count, dispatch] = useReducer(reducer, 1);
```

하나하나 살펴보면 useState를 사용하듯이 배열을 반환하게 되고 첫번째 count는 state이다, 그리고 두번째 dispatch는 상태를 변화시키는 action을 발생시키는 함수이다.
그리고 useReducer를 호출할때 reducer라는 함수를 호출하고 뒤에는 초기값을 지정해주면 된다.  
reducer라는 함수는 dispatch가 상태변화를 일으킬 때 이 상태변화를 reducer가 처리해주는 것이다.  
정리하자면 위와같이 useReducer를 사용해서 count 라는 state를 만들게 되면 초기값이 1로 할당되고 1이라는 값을 변경하고 싶으면 dispatch를 활용해서 상태변화를 일으키면 그것을 reducer가 처리해준다고 생각하면 된다.

```
<button onClick={() => dispatch({ type: 1 })}>add 1</button>
```

이 버튼을 누르게 되면 dispatch함수를 호출하면서 객체로 type: 1을 넘겨주고 있다. 그럼 reducer로 그 값이 날아가게 되는데 reducer는 첫번째 인자로 최신 state값을 첫번째 인자로 받고 두번째 인자 action은 type: 1을 받는다. 그럼 이 action에 따라 switch문에서 할 일들을 정해주고 그걸 리턴해주면 된다.

자 그럼 일기장의 App 컴포넌트를 이 reducer을 사용하여 변화시켜보자 .

```
import React, { useRef, useEffect, useMemo, useCallback, useReducer } from 'react';
import './App.css';
import DiaryEditor from './DiaryEditor';
import DiaryList from './DiaryList';

const reducer = (state, action) => {
    switch (action.type) {
        case 'init': {
            return action.data;
        }
        case 'create': {
            const create_date = new Date().getTime();
            const newItem = {
                ...action.data,
                create_date,
            };
            return [newItem, ...state];
        }
        case 'remove': {
            return state.filter((it) => it.id !== action.targetId);
        }
        case 'edit': {
            return state.map((it) => (it.id === action.targetId ? { ...it, content: action.newContent } : it));
        }
        default:
            return state;
    }
};

function App() {
    // DiaryEditor에서 setData를 사용하여 조작할 수 있는 함수를 props로 넘기고
    // DiaryList에서는 data그대로 넘겨준다.
    // 리액트는 단반향 데이터이고 같은레벨 끼리는 데이터를 전달할 수 없다. 그래서 이벤트를 끌어올리고 데이터는 내려주는 즉 app에서 데이터를 내려주고 조작하는것을 하면 app 아래 같은 레벨끼리 데이터 송수신이 가능하다.

    // 새로운 상태변화 배열 선언
    // const [data, setData] = useState([]);
    // state에서 useReducer로 업그레이드
    const [data, dispatch] = useReducer(reducer, []);

    // useRef를 사용하여 1씩증가하는 값을 만듬
    const dataId = useRef(0);

    // API 호출 함수
    const getData = async () => {
        const res = await fetch('https://jsonplaceholder.typicode.com/comments').then((res) => res.json());

        // 20개 잘라내기
        const initData = res.slice(0, 20).map((it) => {
            return {
                author: it.email,
                content: it.body,
                emotion: Math.floor(Math.random() * 5) + 1,
                create_date: new Date().getTime(),
                id: dataId.current++,
            };
        });

        dispatch({ type: 'init', data: initData });
    };

    // Mount 시점에 API 호출
    useEffect(() => {
        getData();
    }, []);

    // 작성자 내용 감정을 전달받아 새로운 객체로 만들어서 setData를 조작
    const onCreate = useCallback((author, content, emotion) => {
        dispatch({ type: 'create', data: { author, content, emotion, id: dataId.current } });
        dataId.current += 1;
    }, []);

    // 삭제 기능
    const onRemove = useCallback((targetId) => {
        dispatch({ type: 'remove', targetId });
    }, []);

    // 수정기능
    const onEdit = useCallback((targetId, newContent) => {
        dispatch({ type: 'edit', targetId, newContent });
    }, []);

    const getDiaryAnalysis = useMemo(() => {
        const goodCount = data.filter((it) => it.emotion >= 3).length;
        const badCount = data.length - goodCount;
        const goodRatio = (goodCount / data.length) * 100;
        return { goodCount, badCount, goodRatio };
    }, [data.length]);

    const { goodCount, badCount, goodRatio } = getDiaryAnalysis;

    return (
        <div className="App">
            {/* 조작 함수를 내려 줌 */}
            <DiaryEditor onCreate={onCreate} />

            <div>전체 일기 : {data.length} 개</div>
            <div>좋은 일기 : {goodCount} 개</div>
            <div>나쁜 일기 : {badCount} 개</div>
            <div>좋은 일기 비율 : {goodRatio}% </div>

            {/* 데이터만 내려 줌 */}
            <DiaryList onRemove={onRemove} diaryList={data} onEdit={onEdit} />
        </div>
    );
}

export default App;
```
