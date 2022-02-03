# 데이터 삭제하기

## 이전코드 [데이터 추가하기](https://github.com/MyoungSeob-Pohang/daily-TIL/blob/main/React-Toy-Diary/data_add.md)

데이터를 추가했으면 데이터 삭제도 해야한다. 데이터 삭제는 데이터 추가하는 것과 동일하게 state조작 함수를 자식요소에게 내려주면 된다.  
DiaryItem에 삭제하기 버튼을 추가하고, 그 버튼의 onClick 이벤트를 받아 App데이터에서 내려준 state를 조작하면 될거같다.  
일단 DiaryItem컴포넌트에 버튼을 추가하자.

### DiaryItem.js

```
// 값을 받아 옴, spread로 펼쳐서 넘겨줬으니 하나하나 다 받음
const DiaryItem = ({ onDelete, author, content, emotion, create_date, id }) => {
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
            <button
                onClick={() => {
                    if (window.confirm(`${id}번째 일기를 정말 삭제 하시겠습니까?`)) {
                        onDelete(id);
                    }
                }}
            >
                삭제하기
            </button>
        </div>
    );
};

export default DiaryItem;
```

부모컴포넌트에서 받아올 onDelete props를 받아주고 button onClick에 onDelete를 호출하면서 id값을 같이넘겨준다.  
그런데 지금 DiaryItem컴포넌트의 부모는 App이 아니라 DiaryList이다. App으로 바로 갈 수는 없다. 그래서 어쩔 수 없이 DiaryList에서도 onDelete를 받아주어야 한다.

### DiaryList.js

```
import DiaryItem from './DiaryItem';

// 더미데이터를 받음 객체라서 {}
const DiaryList = ({ onDelete, diaryList }) => {
    return (
        <div className="DiaryList">
            <h2>일기 리스트</h2>
            <h4>{diaryList.length}개의 일기가 있습니다.</h4>
            <div>
                {/* 아이디가 없으면 it뒤 두번쨰 인자로 idx를 받아 그대로 키에 사용가능하다. 하지만 추가하거나 삭제해서 idx가 바뀔 경우 문제가 생길 수 있음.*/}
                {diaryList.map((it) => (
                    // 키값을 담고 spread로 props를 DiaryItem넘겨줌
                    <DiaryItem key={it.id} {...it} onDelete={onDelete} />
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

DiaryItem컴포넌트에 props를 넘겨주기 위해 부모인 app에게서 onDelete를 받아오고 다시 자식컴포넌트에게 onDelete={onDelete}로 전달 해주었다.

### App.js

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

    const onDelete = (targetId) => {
        const newDiaryList = data.filter((it) => it.id !== targetId);
        setData(newDiaryList);
    };

    return (
        <div className="App">
            {/* 조작 함수를 내려 줌 */}
            <DiaryEditor onCreate={onCreate} />
            {/* 데이터만 내려 줌 */}
            <DiaryList onDelete={onDelete} diaryList={data} />
        </div>
    );
}

export default App;
```

onDelete함수를 만들어서 Id값을 받아 filter로 id 값을 뺀 결과를 받아 setData에 적용시켜주었다.  
순서는 아래와 같다.

1. DiaryItem에서 삭제버튼 클릭
2. onClick이벤트를 받아 onDelete함수를 호출하면서 id값 전달
3. 부모인 DiaryList를 거쳐 App.js의 onDelete함수 호출
4. data에 filter()를 적용시켜 보낸 아이디와 targetId가 같지 않은 것들만 따로 추려서 필터링 하여 newDiaryList저장 시킨다.
5. setData를 호출하여 newDiaryList값으로 세팅한다.
