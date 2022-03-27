# contextAPI 컴포넌트 트리에 데이터 공급

일기장의 구조 형식을 보자면 최상위 부모인 App컴포넌트 아래에 DiaryEditor컴포넌트와 DiaryList컴포넌트가 존재하고 DiaryList컴포넌트는 아래에는 DiaryItem컴포넌트가 존재한다. 그리고 App은 Editor에 onCreate를 props로 넘겨주고 DiaryList에는 diaryList, onRemove, onEdit prop를 전달해준다. 그럼 이 DiaryList가 자식인 DiaryItem에게 onRemove, onEdit를 전달해준다. 근데 자세히 보면 DiaryList는 사용하지도 않는 onRemove, onEdit를 자식요소를 위해 받아야 한다는 것이 비효율 적인것 같다. 만약 onRemove가 onDelete로 이름이 바뀐다고 하면 모두 찾아서 저 이름을 바꿔줘야 해서 귀찮고 오류가 날 수도있을 것이기 때문에 이 문제를 해결하고 싶다. 그리고 이러한 상황을 props가 드릴처럼 땅을파고 들어간다고 하여 props drilling이라고 하기도 한다.  
이러한 문제를 해결하기 위해 Context가 생겻는데 이론은 모든 데이터를 가지고 있는 Provider이라는 공급자 역할을 하는 컴포넌트에게 App이 모든데이터를 주고 이 Provider는 좀 특별해서 자기 자손에 해당하는 모든 컴포넌트에게 직통으로 데이터를 줄 수 있다. 굳이 거쳐가야 하는게 없어졌다는 뜻이다. Provider가 onRemove를 DiaryItem에 바로 줄 수 있게 된다는 것이다. 방법은 아래와 같다.

1. Context 생성

```
const MyContext = React.createContext(defaultValue);
```

2. Context Provider를 통한 데이터 공급

```
<MyContext.Provider value={전역으로 전달 하고자 하는 값}>
    <!-- 이 Context안에 위치할 자식 컴포넌트 들 -->
</MyContext.Provider>
```

그럼 전체 코드는 아래와 같이 변경이 된다.

1. App.js

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

export const DiaryStateContext = React.createContext();
export const DiaryDispatchContext = React.createContext();

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

    const memoizedDispatches = useMemo(() => {
        return { onCreate, onRemove, onEdit };
    }, []);

    return (
        <DiaryStateContext.Provider value={data}>
            <DiaryDispatchContext.Provider value={memoizedDispatches}>
                <div className="App">
                    <DiaryEditor />

                    <div>전체 일기 : {data.length} 개</div>
                    <div>좋은 일기 : {goodCount} 개</div>
                    <div>나쁜 일기 : {badCount} 개</div>
                    <div>좋은 일기 비율 : {goodRatio}% </div>

                    <DiaryList />
                </div>
            </DiaryDispatchContext.Provider>
        </DiaryStateContext.Provider>
    );
}

export default App;
```

2. DiaryEditor.js

```
// useRef 는 요소를 선택할 수 있는 주소값을 가진다.
// useState는 상태를 관리할 수 있게 해준다.
// 모두 react에 포함되어 있어서 함께 import
import React, { useRef, useState, useEffect, useMemo, useContext } from 'react';
import { DiaryDispatchContext } from './App';

// DiaryEditor 함수는 전체를 감싸는 함수 이다.
// 파일명과 같으며 함수를 실행한다.
// 데이터조작 함수를 받음
const DiaryEditor = () => {
    const { onCreate } = useContext(DiaryDispatchContext);

    useEffect(() => {
        console.log('에디터 렌더');
    });

    // 작성자 Ref
    const authorInput = useRef();
    // 컨텐츠(일기내용) Ref
    const contentInput = useRef();

    // 전체 상태관리를 위한 state
    // 객체로 한번에 관리할 수 있다.
    // key : value 값으로 관리하며 value는 초기값에 해당한다.
    const [state, setState] = useState({
        author: '',
        content: '',
        emotion: 1,
    });

    // 이벤트 함수
    // 스프레드로 전체 상태값을 받아오고 선택한 태그의 name값을 가져와서 대괄호 표기법으로 접근해서 값을 변경시킨다.
    // name 값은 useState 의 선언된 key값과 동일함으로 접근 가능하다.
    const handleChangeState = (e) => {
        setState({
            ...state,
            [e.target.name]: e.target.value,
        });
    };

    // 클릭이벤트 함수
    // 제목의 길이와 컨텐츠의 길이가 일정글자 수 이하이면 focus를 준다.
    const handleSubmit = (e) => {
        if (state.author.length < 1) {
            authorInput.current.focus();
            return;
        }

        if (state.content.length < 5) {
            contentInput.current.focus();
            return;
        }

        // 전달받은 함수를 호출하여 값을 저장시킴
        onCreate(state.author, state.content, state.emotion);
        alert('저장 성공');
        // 저장 후 초기값 세팅
        setState({
            author: '',
            content: '',
            emotion: 1,
        });
    };

    // 렌더링 되는 html
    // state가 객체로 선언되어 있기 때문에 state.key 로 접근이 가능하다.
    return (
        <div className="DiaryEditor">
            <h2>오늘의 일기</h2>
            <div>
                <input ref={authorInput} name="author" value={state.author} onChange={handleChangeState} />
            </div>
            <div>
                <textarea ref={contentInput} name="content" value={state.content} onChange={handleChangeState} />
            </div>
            <div>
                <select name="emotion" value={state.emotion} onChange={handleChangeState}>
                    <option value={1}>1</option>
                    <option value={2}>2</option>
                    <option value={3}>3</option>
                    <option value={4}>4</option>
                    <option value={5}>5</option>
                </select>
            </div>
            <div>
                <button onClick={handleSubmit}>일기 저장하기</button>
            </div>
        </div>
    );
};

// memo로 묶음
export default React.memo(DiaryEditor);
```

3. DiaryList.js

```
import { useContext } from 'react';
import { DiaryStateContext } from './App';
import DiaryItem from './DiaryItem';

// 더미데이터를 받음 객체라서 {}
const DiaryList = () => {
    const diaryList = useContext(DiaryStateContext);
    return (
        <div className="DiaryList">
            <h2>일기 리스트</h2>
            <h4>{diaryList.length}개의 일기가 있습니다.</h4>
            <div>
                {/* 아이디가 없으면 it뒤 두번쨰 인자로 idx를 받아 그대로 키에 사용가능하다. 하지만 추가하거나 삭제해서 idx가 바뀔 경우 문제가 생길 수 있음.*/}
                {diaryList.map((it) => (
                    // 키값을 담고 spread로 props를 DiaryItem넘겨줌
                    <DiaryItem key={it.id} {...it} />
                ))}
            </div>
        </div>
    );
};

// 기본 값 설정
DiaryList.defaultProps = {
    diaryList: [],
};

export default DiaryList;
```

4. DiaryItem.js

```
import React, { useContext, useEffect, useRef, useState } from 'react';
import { DiaryDispatchContext } from './App';

// 값을 받아 옴, spread로 펼쳐서 넘겨줬으니 하나하나 다 받음
const DiaryItem = ({ author, content, emotion, create_date, id }) => {
    const { onRemove, onEdit } = useContext(DiaryDispatchContext);

    // 수정 중, 수정 중이 아님의 상태를 저장할 State
    const [isEdit, setIsEdit] = useState(false);
    // toggleIsEdit를 호출하여 isEdit의 값을 반전시킴
    const toggleIsEdit = () => setIsEdit(!isEdit);
    // 수정하기 버튼을 눌렀을 때 나오는 textarea 관리를 위한 State
    const [localContent, setLocalContent] = useState(content);
    // 수정하기 버튼을 눌렀을 때 나오는 textarea의 Ref
    const localContentInput = useRef();

    // 삭제하기 클릭이벤트
    const handleRemove = () => {
        if (window.confirm(`${id}번째 일기를 정말 삭제 하시겠습니까?`)) {
            onRemove(id);
        }
    };

    // textarea수정 중 내용을 작성하다가 취소를 누르면 기존값이 남아있다. 그 기존값 초기화를 위한 함수
    const handleQuitEdit = () => {
        setIsEdit(false);
        setLocalContent(content);
    };

    // 수정완료 이벤트 , App에 onEdit을 호출하여 값을 변경시킴
    const handleEdit = () => {
        if (localContent.length < 5) {
            localContentInput.current.focus();
            return;
        }
        if (window.confirm(`${id}번째 일기를 수정하시겠습니까 ?`)) {
            onEdit(id, localContent);
            toggleIsEdit();
        }
    };

    return (
        <div className="DiaryItem">
            <div className="info">
                <span>
                    작성자 {author} | 감정점수 : {emotion}
                </span>
                <br />
                {/* toLocaleDateString : 사용문화권에 맞는 시간표기법으로 객체의 시간을 리턴 / 운영체제에 설정된 시간표기법 */}
                <span className="date">{new Date(create_date).toLocaleDateString()}</span>
            </div>
            <div className="content">
                {isEdit ? (
                    <>
                        <textarea ref={localContentInput} value={localContent} onChange={(e) => setLocalContent(e.target.value)}></textarea>
                    </>
                ) : (
                    <>{content}</>
                )}
            </div>
            {isEdit ? (
                <>
                    <button onClick={handleEdit}>완료</button>
                    <button onClick={handleQuitEdit}>취소</button>
                </>
            ) : (
                <>
                    <button onClick={handleRemove}>삭제</button>
                    {/* toggleIsEdit를 호출하여 수정할지 안할지를 결정해야 함으로 수정하기 버튼에 연결 */}
                    <button onClick={toggleIsEdit}>수정</button>
                </>
            )}
        </div>
    );
};

export default React.memo(DiaryItem);
```
