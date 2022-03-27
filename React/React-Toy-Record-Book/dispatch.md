# dispatch

기본셋팅에서 이어서 App.js를 마저 작성하였다. useReducer를 활용하여 글작성, 삭제, 수정 함수를 만들어 주었다.  
솔직히 제대로 작동할지 안할지 몰라서 차후 수정이 필요할 수도있지만 일단 배운대로 작성해 두 었다.  
아 그리고 context작성할 때 Provider를 작성하지 않아서 에러가 났는데 이거 찾는데 10분 걸렸다. Provider도 추가해 주었다.
전체코드는 아래와 같다. 그리고 onCreate 함수는 차후 다시 사용할 수도 있을 듯해서 useCallback을 사용해서 묶어주었다.

```
import React, { useCallback, useReducer, useRef } from 'react';
import { BrowserRouter, Route, Routes } from 'react-router-dom';
import Main from './pages/Main';
import New from './pages/New';
import Edit from './pages/Edit';
import Record from './pages/Record';
import './App.css';

// 전체 Data상태값전달을 위한 Context
export const RecordStateContext = React.createContext();
// Data상태값 변경 함수전달을 위한 Context
export const RecordDispatchContext = React.createContext();

// 상태와 액션을 받아와 새로운 형태로 반환하는 함수
const reducer = (state, action) => {
    switch (action.type) {
        case 'INIT': {
            // 그대로
            return action.data;
        }
        case 'CREATE': {
            // 새로 받아온 data 넣고 기존 data 추가
            return [action.data, ...state];
        }
        case 'REMOVE': {
            // id 다른 것만 새로운 배열로 받아서 리턴
            return state.filter((it) => it.id !== action.targetId);
        }
        case 'EDIT': {
            // 아이디 같은 data 찾아서 콘텐츠 업데이트
            return state.map((it) => (it.id === action.targetId ? { ...action.data } : it));
        }
        default:
            return state;
    }
};

function App() {
    // 전체 Data를 관리하기 위한 상태값, 초기값은 []빈 배열을 지정
    const [data, dispatch] = useReducer(reducer, []);
    // 아이디값으로 사용하기 위한 선언
    const dataId = useRef(0);

    // 새로운 글 작성
    const onCreate = useCallback((date, comment, studyTime, subject) => {
        dispatch({ type: 'CREATE', data: { date, comment, studyTime, subject, id: dataId.current } });
        dataId.current += 1;
    }, []);

    // 글 삭제
    const onRemove = (targetId) => {
        dispatch({ type: 'REMOVE', targetId });
    };

    // 글 수정
    const onEdit = (targetId, comment, studyTime, subject) => {
        dispatch({ type: 'EDIT', data: { targetId, comment, studyTime, subject } });
    };

    return (
        // 전체 Data상태값전달을 위한 Context
        <RecordStateContext.Provider value={data}>
            {/* Data상태값 변경 함수전달을 위한 Context */}
            <RecordDispatchContext.Provider value={{ onCreate, onRemove, onEdit }}>
                {/* 전체 라우터 */}
                <BrowserRouter>
                    <div className="App">
                        <Routes>
                            {/* path를 지정하는 라우트 :id와 같이 변수로 지정하여 각 게시글에 id값을 받아올 예정*/}
                            <Route path="/" element={<Main />} />
                            <Route path="/new" element={<New />} />
                            <Route path="/edit/:id" element={<Edit />} />
                            <Route path="/record/:id" element={<Record />} />
                        </Routes>
                    </div>
                </BrowserRouter>
            </RecordDispatchContext.Provider>
        </RecordStateContext.Provider>
    );
}

export default App;
```
