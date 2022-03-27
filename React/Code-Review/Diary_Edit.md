# Edit

[New Diary](https://github.com/MyoungSeob-Pohang/daily-TIL/blob/main/Code-Review/New_Diary.md) 에서 이어지는 문서

이번에는 일기 수정 컴포넌트 구현이다. 위 New Diary에서 구현한 DiaryEditor컴포넌트를 그대로 승계받아서 사용한다.  
DiaryEditor컴포넌트는 새로운 일기쓰기와 거의 유사하며 안에 내용만 몇개 달라지는 컴포넌트이기 때문이다.

## Edit.js

```
import { useContext, useEffect, useState } from 'react';
import { useNavigate, useParams } from 'react-router-dom';
import { DiaryStateContext } from '../App';
import DiaryEditor from '../components/DiaryEditor';

const Edit = () => {
    const navigate = useNavigate();
    const { id } = useParams();
    const diaryList = useContext(DiaryStateContext);
    const [originData, setOriginData] = useState();

    useEffect(() => {
        if (diaryList.length >= 1) {
            const targetDiary = diaryList.find((it) => parseInt(it.id) === parseInt(id));

            if (targetDiary) {
                setOriginData(targetDiary);
            } else {
                navigate('/', { replace: true });
            }
        }
    }, [id, diaryList, navigate]);

    return <div className="Edit">{originData && <DiaryEditor isEdit={true} originData={originData} />}</div>;
};

export default Edit;
```

1. Navigate를 사용하기 위한 선언

```
const navigate = useNavigate();
```

2. useParams로 url query값을 가지고 온다.

```
const { id } = useParams();
```

3. 전체 일기리스트를 가지고 오기 위해 App에서 선언된 Context로 data 값을 가지고 오기 위한 선언

```
const diaryList = useContext(DiaryStateContext);
```

4. 원본 데이터 상태를 위한 선언

```
const [originData, setOriginData] = useState();
```

5. useEffect로 id, diaryList, navigate 값이 변경될 떄 마다 실행할 함수를 만든다.
   이 함수는 useContext로 가져온 일기데이터의 값이 1개 이상일 때 그 값에 접근하여 url query로 넘어온 아이디와 비교를 하여 같은 일기데이터를 찾는다.
   그리고 일기데이터가 있다면 setOriginData으로 originData의 값을 변경시켜주고 없다면 홈으로 보내고 뒤로가기를 막는다.
   현재 더미데이터는 아래와 같다. 그리고 라우팅 되는 부분은 <Route path="/edit/:id" element={<Edit />} /> 이다.  
   그럼 .../edit/1 부터 .../edit/5 까지 정보가 넘어올 수 있고 나머지는 id가 존재하지 않음으로 결국 수정할 일기데이터가 없기 때문에 홈으로 보낸 후 뒤로가기가 안되게 막은 것이다.

```
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
```

```
useEffect(() => {
    if (diaryList.length >= 1) {
        const targetDiary = diaryList.find((it) => parseInt(it.id) === parseInt(id));

        if (targetDiary) {
            setOriginData(targetDiary);
        } else {
            navigate('/', { replace: true });
        }
    }
}, [id, diaryList, navigate]);
```

6. originData가 있으면 DiaryEditor컴포넌트에 isEdit={true} originData={originData}의 값을 넘겨준다.

```
return <div className="Edit">{originData && <DiaryEditor isEdit={true} originData={originData} />}</div>;
```

7. DiaryEditor 컴포넌트에서 isEdit값과 originData의 값을 props로 받아준다.

```
const DiaryEditor = ({ isEdit, originData }) => {
```

8. useEffect를 사용하여 isEdit와 originData가 변경될 때 isEdit가 true면 date값과 감정, 내용 상태를 각각 바꾸어 준다. 결국 isEdit와 originData는 무조건 Edit컴포넌트에서 날아오는 값임으로 수정시에만 실행된다고 생각하면 된다.

```
useEffect(() => {
    if (isEdit) {
        setDate(getStringDate(new Date(parseInt(originData.date))));
        setEmotion(originData.emotion);
        setContent(originData.content);
    }
}, [isEdit, originData]);
```

9. 작성완료가 보내는 onClick 함수 handleSubmit에 logic를 약간 바꿔준다. isEdit 값에 따라서 comfirm의 값을 다르게 하고 수행하는 함수도 다르게한다.  
   isEdit가 true일 때 즉 수정하기 버튼을 눌러서 작성한 값을 눌럿을 때는 일기를 수정하겠습니까 멘트를 띄우고 true를 사용자가 누르면 onEdit함수가 실행된다.
   그게 아니면 (새로운 글을 작성할 때) onCreate함수를 실행한다.

```
const handleSubmit = () => {
    if (content.length < 1) {
        contentRef.current.focus();
        return;
    }

    if (window.confirm(isEdit ? '일기를 수정 하시겠습니까?' : '일기를 작성 하시겠습니까?')) {
        if (isEdit) {
            onEdit(originData.id, date, content, emotion);
        } else {
            onCreate(date, content, emotion);
        }
    }

    navigator('/', { replace: true });
};
```
