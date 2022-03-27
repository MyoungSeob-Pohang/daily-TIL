# 상세 Diary 코드 상세 리뷰 < 상태관리 분리 >

## useReducer

보통 상태관리를 할 때 useState를 주로 사용하지만, 하지만 useState를 사용하면 만약 관리해야 할 State가 많아진다면, 복잡하게 보일 수 있다.  
이럴 때 좀더 쉽게 관리하기 위해 useReducer라는 Hook이 존재한다.

```
const [data, dispatch] = useReducer(reducer, []);
```

useReducer는 useState와 비슷하지만 약간 다르다.

1. data : useState와 동일하게 사용할 상태의 값이다.

2. dispatch : 컴포넌트 내에서 상태 변경을 일으키기 위해서 사용하는 값. 인자로 reducer 함수에 넘길 행동(action) 객체를 받는다. 컴포넌트에서 dispatch 함수에 행동(action)을 던지면, reducer 함수가 이 행동(action)에 따라서 상태(state)를 변경해준다.

3. reducer : dispatch가 보내준 값에 따라 어떻게 변경 시켜야할 지 정의한다.

4. [] : 지금은 빈 배열로 넣었지만 data의 초기값을 설정한다.

언뜻 보면 복잡해 보이지만 그냥 사용할 때 dispatch에 type과 data를 정의해서 객체로 호출하게 되면 그 type과 data를 reducer함수가 받아서 type에 따라 어떤 행동을 할지 결정하는 것이다.

그러면 reducer함수를 이제 정의해야 한다.

```
const reducer = (state, action) => {
    let newState = [];

    switch (action.type) {
        case 'INIT': {
            return action.data;
        }
        case 'CREATE': {
            newState = [action.data, ...state];
            break;
        }
        case 'REMOVE': {
            newState = state.filter((it) => it.id !== action.targetId);
            break;
        }
        case 'EDIT': {
            newState = state.map((it) => (it.id === action.data.id ? { ...action.data } : it));
            break;
        }
        default:
            return state;
    }
    return newState;
};
```

switch문의로 작성이 되어 있다. 함수만 봐서는 이해가 되지 않으니 호출문까지 같이 보도록 하자.

```
dispatch({ type: 'CREATE', data: { id: dataId.current, date: new Date(date).getTime(), content, emotion } });
```

이 코드는 Create(일기작성)을 하기 위해 dispatch를 호출하는 것인데 보면 객체로 type와 data를 넘겨주고 있다.  
그럼 이 dispatch는 요 값들을 가지고 reducer함수를 호출한다.  
reducer함수는 2가지 값을 받는데 첫번쨰 값은 state 두번째는 action이다. 이 type와 data는 action에 값으로 전달된다.
그럼 state는 무엇이냐, 바로 data의 최신 정보들이다.

1. dispatch로 함수호출을 하면 호출 때 data의 최신 값이 state에 전달 되고, action에 dispatch에서 보내온 객체가 전달된다.
2. switch문에서 action.type를 받는다. 위 코드를 예시로 들면 CREATE가 전달된다.

```
case 'CREATE': {
    newState = [action.data, ...state];
    break;
}
```

3. 위 코드가 실행된다.
4. newState라는 변수에 배열로 0번 인덱스에는 action의 data들이 위치하고 -> data: { id: dataId.current, date: new Date(date).getTime(), content, emotion }
5. 1번 인덱스에는 data의 최신 값 전체가 전달된다. 그리고 switch문을 종료한다. break
6. 변경된 상태인 newState를 리턴한다.

전체코드

```
// 실시간 state와 행동을 담은 action
const reducer = (state, action) => {
    // 새로운 상태 담을 배열
    let newState = [];

    console.log(state);
    console.log(action);

    switch (action.type) {
        case 'INIT': {
            return action.data;
        }
        // data: { id: dataId.current, date: new Date(date).getTime(), content, emotion } 을 받아오니, 새로운 상태에 그대로 담고 나머진 state그대로
        case 'CREATE': {
            newState = [action.data, ...state];
            break;
        }
        // targetId를 받아와서 해당하는 아이디가 아닌 것만 모두 골라서 새로운 상태에 저장
        case 'REMOVE': {
            newState = state.filter((it) => it.id !== action.targetId);
            break;
        }
        // data: { id: targetId, date: new Date(date).getTime(), content, emotion } 같은 아이디를 가졌으면 업데이트 시키고 아니면 그대로 반환
        case 'EDIT': {
            newState = state.map((it) => (it.id === action.data.id ? { ...action.data } : it));
            break;
        }
        default:
            return state;
    }
    // switch 끝나고 새로운 상태 리턴
    return newState;
};

const dummyData = [
    {
        id: 1,
        emotion: 1,
        content: '오늘의일기 1번',
        date: 1644241008374,
    },
    {
        id: 2,
        emotion: 2,
        content: '오늘의일기 2번',
        date: 1644241008376,
    },
    {
        id: 3,
        emotion: 3,
        content: '오늘의일기 3번',
        date: 1644241008379,
    },
    {
        id: 4,
        emotion: 4,
        content: '오늘의일기 4번',
        date: 1644241008380,
    },
    {
        id: 5,
        emotion: 5,
        content: '오늘의일기 5번',
        date: 1644241008382,
    },
];

// 상태 업데이트 로직 분리를 위한 useReducer 선언 , 초기값으로 dummyData 전달
const [data, dispatch] = useReducer(reducer, dummyData);

// 아이디 값으로 쓰기위한 Ref
const dataId = useRef(0);

// CREATE
// 작성일, 내용, 감정을 받아서 객채형식으로 전달.
const onCreate = (date, content, emotion) => {
    dispatch({ type: 'CREATE', data: { id: dataId.current, date: new Date(date).getTime(), content, emotion } });
    // 아이디 값 1 증가
    dataId.current += 1;
};

// REMOVE
// 아이디 값을 받아 아이디 값 그대로 전달
const onRemove = (targetId) => {
    dispatch({ type: 'REMOVE', targetId });
};

// EDIT
// 아이디, 날짜, 내용, 감정을 모두 받아 전달
const onEdit = (targetId, date, content, emotion) => {
    dispatch({ type: 'EDIT', data: { id: targetId, date: new Date(date).getTime(), content, emotion } });
};
```
