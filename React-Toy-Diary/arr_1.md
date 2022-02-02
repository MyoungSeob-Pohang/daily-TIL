# React에서 list 사용하기

대부분의 웹사이트는 배열에 아이템을 저장해서 리스트 형태로 렌더링 하는 사례가 많은데, 배열을 이용하여 list를 렌더링 해보고 그것을 개별적인 컴포넌트로 만들어보려고 한다.
일기 작성자 내용, 작성일 등을 배열의 객체에 담아 데이터를 컴포넌트에 넘겨 받으면서 출력하는 예제이다.

우선 dummy를 생성해보자 ,

```
const dummyList = [
    {
        id: 1,
        author: 'moonsbi',
        content: 'Hello World!',
        emotion: 1,
        create_date: new Date().getTime(),
    },
    {
        id: 2,
        author: '홍길동',
        content: 'Hello 경상북도,
        emotion: 2,
        create_date: new Date().getTime(),
    },
    {
        id: 3,
        author: '바보',
        content: 'Hello Pohang',
        emotion: 3,
        create_date: new Date().getTime(),
    },
];
```

더미데이터를 배열로 선언 후 각 아이템은 객체로 선언해 주었다. 고유한 id값과 작성자, 내용, 기분, 그리고 작성시간은 현재시간 기준으로 작성하였다.
이 더미데이터를 DiaryList라는 컴포넌트를 만들어서 diaryList라는 이름으로 값을 넘겨주었다.

```
{/* 더미데이터 데이터를 넘겨줌  */}
<DiaryList diaryList={dummyList} />
```

그럼 DiaryList라는 컴포넌트를 만들어서 아래와 같이 사용할 수 있다.

```
DiaryList.js

import DiaryItem from './DiaryItem';

const DiaryList = ({ diaryList }) => {
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

일단 DiaryList에서 넘어온 값을 사용하기 위해 객체 { diaryList }로 값을 받았다.  
그 객체에 점표기법(.)으로 접근하여 diaryList.length로 총 몇개의 값이 넘어왔는지 알 수 있다.  
조금 더 유연한 컴포넌트 작성을 위해 DiaryItem컴포넌트를 새로 만들어서 그 안에서 데이터를 조작하기로 하였다. DiaryItem컴포넌트를 만들때 map을 사용하였는데, 이 map은 주어진 값들을 순회하면서 새로운 배열을 반환한다. 그렇기에 it은 모든 객체를 순회하면서 DiaryItem의 key값을 설정 하고 spread로 모든 정보를 넘겨주었다.  
그럼 DiaryItem에서 아래와 같이 받을 수 있다.

```
DiaryItem.js

// 값을 받아 옴
const DiaryItem = ({ author, content, emotion, create_date, id }) => {
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
            <div className="content">{content}</div>
        </div>
    );
};

export default DiaryItem;
```

전체 코드는 아래와 같다.

1. App.js

```
import './App.css';
import DiaryEditor from './DiaryEditor';
import DiaryList from './DiaryList';

// 더미데이터
const dummyList = [
    {
        id: 1,
        author: 'moonsbi',
        content: 'Hello World! - 1',
        emotion: 1,
        create_date: new Date().getTime(),
    },
    {
        id: 2,
        author: '홍길동',
        content: 'Hello World! - 2',
        emotion: 2,
        create_date: new Date().getTime(),
    },
    {
        id: 3,
        author: '뮨스비',
        content: 'Hello World! - 3',
        emotion: 3,
        create_date: new Date().getTime(),
    },
];

const App = () => {
    return (
        <div className="App">
            <DiaryEditor />
            {/* 더미데이터를 props로 넘겨줌  */}
            <DiaryList diaryList={dummyList} />
        </div>
    );
};

export default App;
```

2. DiaryEditor.js

```
// useRef 는 요소를 선택할 수 있는 주소값을 가진다.
// useState는 상태를 관리할 수 있게 해준다.
// 모두 react에 포함되어 있어서 함께 import
import { useRef, useState } from 'react';

// DiaryEditor 함수는 전체를 감싸는 함수 이다.
// 파일명과 같으며 함수를 실행한다.
const DiaryEditor = () => {
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

        alert('저장 성공');
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

export default DiaryEditor;
```

3. DiaryList.js

```
import DiaryItem from './DiaryItem';

// 더미데이터를 받음 객체라서 {}
const DiaryList = ({ diaryList }) => {
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
// 값을 받아 옴, spread로 펼쳐서 넘겨줬으니 하나하나 다 받음
const DiaryItem = ({ author, content, emotion, create_date, id }) => {
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
            <div className="content">{content}</div>
        </div>
    );
};

export default DiaryItem;
```
