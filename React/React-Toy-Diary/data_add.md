# 데이터 추가하기

[리스트 렌더링](https://github.com/MyoungSeob-Pohang/daily-TIL/blob/main/React-Toy-Diary/arr_1.md) 의 코드와 같이 현재 구조를 살펴보면,  
App아래로 작성폼인 DiaryEditor과 리스트폼인 DiaryList가 존재한다. App컴포넌트가 부모이고 DiaryEditor / DiaryList 가 자식인 셈이다.  
리액트로 개발을 진행하다 보면은 이런 부모,자식구조가 얽힌 트리구조 컴포넌트가 만들어진다.  
DiaryEditor에서 일기 아이템을 추가하면 DiaryList에서 나와야 하는데 리액트에서 같은레벨 끼리 데이터를 주고받을 수 없다. 즉 우리는 App이 0번 레벨 DiaryEditor / DiaryList는 각 1레벨 인 것이다.  
현재 App에서 dummyList를 DiaryList에 내려주고 DiaryList는 그것을 사용하였는데, 이것이 리액트는 단방향으로만 데이터가 흐른다 라고 표현한다.

그럼 실제로 DiaryEditor의 값을 DiaryList에 추가할 수 없을까 ? 이럴때는 리액트의 상태요소를 공통부모인 App 으로 끌어올려서 해결할 수 있다.  
App컴포넌트가 일기데이터를 state형태 배열로 가지고 있고 이 data의 값을 DiaryList에 주고 이 data의 값을 변화시킬 수 있는 setData를 DiaryEditor컴포넌트에 props로 넘겨주면 된다.  
그럼 만약 DiaryEditor에서 setData로 접근해서 값을 바꾸게 되면 그 바뀐값을 App은 DiaryList에 전달해 주게 되고 DiaryList는 rerender가 되면서 바뀐 값을 출력하게 된다.  
이 방법을 State끌어올리기 라고한다.

그럼 코드를 하나하나 수정해보자.

1. App.js

```
import { useRef, useState } from 'react/cjs/react.development';
import './App.css';
import DiaryEditor from './DiaryEditor';
import DiaryList from './DiaryList';

function App() {
    // DiaryEditor에서 setData를 사용하여 조작할 수 있는 함수를 props로 넘기고
    // DiaryList에서는 data그대로 넘겨준다.
    // 리액트는 단반향 데이터이고 같은레벨 끼리는 데이터를 전달할 수 없다. 그래서 이벤트를 끌어올리고 데이터는 내려주는 즉 app에서 데이터를 내려주고 조작하는것을 하면 app 아래 같은 레벨끼리 데이터 송수신이 가능하다.

    // 새로운 상태변화 배열 선언
    const [data, setData] = useState([]);
    // useRef를 사용하여 1씩증가하는 값을 만듬
    const dataId = useRef(0);

    // 작성자 내용 감정을 전달받아 새로운 객체로 만들어서 setData를 조작
    const onCreate = (author, content, emotion) => {
        const create_date = new Date().getTime();

        const newItem = {
            author,
            content,
            emotion,
            create_date,
            id: dataId.current,
        };
        dataId.current += 1;
        setData([newItem, ...data]);
    };
    return (
        <div className="App">
            {/* 조작 함수를 내려 줌 */}
            <DiaryEditor onCreate={onCreate} />
            {/* 데이터만 내려 줌 */}
            <DiaryList diaryList={data} />
        </div>
    );
}

export default App;
```

위 코드를 보면 상태변화를 위한 새로운 useState를 선언하고 빈 배열로 초기화 하고있다. 그리고 useRef(0)을 통해서 부여할 id 초기값을 만들어 주었다.

onCreate라는 함수를 만들어서 작성자, 내용, 감정을 받고 새로운 시간을 선언 후 newItem 이라는 새로운 객체에 담아두었다. 담는 작업이 끝나면 dataId를 +1 하고 setData에 접근하여 새로운 객체로 업데이트를 하였다.

그리고 DiaryEditor에 onCreate 함수를 내려주었다. onCreate함수는 작성자 내용 감정을 전달받아 setData에 변경하는 함수이다.  
그럼 DiaryEditor컴포넌트로 가보자.

2. DiaryEditor.js

```
// useRef 는 요소를 선택할 수 있는 주소값을 가진다.
// useState는 상태를 관리할 수 있게 해준다.
// 모두 react에 포함되어 있어서 함께 import
import { useRef, useState } from 'react';

// DiaryEditor 함수는 전체를 감싸는 함수 이다.
// 파일명과 같으며 함수를 실행한다.
// 데이터조작 함수를 받음
const DiaryEditor = ({ onCreate }) => {
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

export default DiaryEditor;
```

const DiaryEditor = ({ onCreate }) 에서 넘겨준 값을 받아온다. 그리고 handleSubmit함수가 저장버튼을 누를 때 호출되는 함수이니,  
누를때 전체 정보를 onCreate를 호출하여 값을 넘겨준다. 그리고 저장에 성공한 후 setState에 접근하여 다시 빈값으로 초기화를 해준다.
