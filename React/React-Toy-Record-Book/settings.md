# 기본세팅

1. src안에 components라는 폴더를 만들어준다.
   이 폴더는 페이지에서 불러오는 자식 컴포넌트들이 위치할 예정이다.

2. src안에 pages라는 폴더를 만들어준다.  
   이 폴더는 각 최상위 컴포넌트가 되는 컴포넌트를 배치하였다.  
   글을 작성하는 New, 글을 수정하는 Edit, 글을 읽을 수 있는 Record, 글 전체 리스트이며 메인페이지가 되는 Main

3. createContext로 createContext2개 생성
   RecordStateContext는 전체 상태값을 가지는 data를 내려줄 컨텍스트이며, RecordDispatchContext는 글작성, 수정, 삭제 등을 할 때 필요한 함수를 자식에게 내려주는 컨텍스트이다.

4. const [data, dispatch] = useReducer(reducer, []); 전체 데이터를 관리할 상태를 선언해주고 useReducer로 역할을 분리하여 준다.

5. Route 세팅을 해준다.

6. react-router-dom을 사용하기 위해 npm install react-router-dom@6 으로 설치하여 준다. @6은 버전이다

위 5가지 세팅을 하면 아래와 같은 코드가 나온다. 주석에 설명을 달아놨으니 한번 더 확인하자.

```
App.js

import React, { useReducer, useRef } from 'react';
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
    return;
};

function App() {
    // 전체 Data를 관리하기 위한 상태값, 초기값은 []빈 배열을 지정
    const [data, dispatch] = useReducer(reducer, []);
    // 아이디값으로 사용하기 위한 선언
    const dataId = useRef(0);

    return (
        // 전체 Data상태값전달을 위한 Context
        <RecordStateContext value={data}>
            {/* Data상태값 변경 함수전달을 위한 Context */}
            <RecordDispatchContext>
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
            </RecordDispatchContext>
        </RecordStateContext>
    );
}

export default App;
```

폴더구조는 아래와 같다.

```
node_modules
public
src
....components
....pages
........Edit.js
........Main.js
........New.js
........Record.js
....util
....App.css
....App.js
....index.css
....index.js
.gitignore
package-look.json
package.json
README.md
yarn.lock
```
