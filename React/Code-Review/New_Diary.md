# New_Diary

[Diary List](https://github.com/MyoungSeob-Pohang/daily-TIL/blob/main/Code-Review/Diary_List.md) 에서 이어지는 문서

이번엔 새로운 일기를 추가하는 부분이다. 코드 전체를 보면서 하나씩 해석하면서 보자. 컴포넌트는 총 3개로 이루어져 있고,  
New컴포넌트는 DiaryEditor컴포넌트를, DiaryEditor컴포넌트는 EmotionItem컴포넌트를 호출한다.

1. New 컴포넌트

```
import DiaryEditor from '../components/DiaryEditor';

const New = () => {
    return (
        <div>
            <DiaryEditor />
        </div>
    );
};

export default New;
```

단순히 DiaryEditor컴포넌트를 호출한다. 코드의 구분을 쉽게하기 위해 컴포넌트를 분리하였다.

2. DiaryEditor 컴포넌트

```
import MyHeader from './MyHeader';
import MyButton from './MyButton';
import { useNavigate } from 'react-router-dom';
import { useContext, useRef, useState } from 'react';
import EmotionItem from './EmotionItem';
import { DiaryDispatchContext } from '../App';

const getStringDate = (date) => {
    return date.toISOString().slice(0, 10);
};

const emotionList = [
    { emotion_id: 1, emotion_img: process.env.PUBLIC_URL + '/assets/emotion1.png', emotion_descript: '완전 좋음' },
    { emotion_id: 2, emotion_img: process.env.PUBLIC_URL + '/assets/emotion2.png', emotion_descript: '좋음' },
    { emotion_id: 3, emotion_img: process.env.PUBLIC_URL + '/assets/emotion3.png', emotion_descript: '그럭저럭' },
    { emotion_id: 4, emotion_img: process.env.PUBLIC_URL + '/assets/emotion4.png', emotion_descript: '나쁨' },
    { emotion_id: 5, emotion_img: process.env.PUBLIC_URL + '/assets/emotion5.png', emotion_descript: '끔찍함' },
];

const DiaryEditor = () => {
    const navigator = useNavigate();
    const [date, setDate] = useState(getStringDate(new Date()));
    const [emotion, setEmotion] = useState(3);
    const [content, setContent] = useState();
    const contentRef = useRef();
    const { onCreate } = useContext(DiaryDispatchContext);

    const handleClickEmotion = (emotion) => {
        setEmotion(emotion);
    };

    const handleSubmit = () => {
        if (content.length < 1) {
            contentRef.current.focus();
            return;
        }
        onCreate(date, content, emotion);
        navigator('/', { replace: true });
    };
    return (
        <div className="DiaryEditor">
            <MyHeader headText={'새 일기쓰기'} leftChild={<MyButton text={'< 뒤로가기'} onClick={() => navigator(-1)} />} />
            <div>
                <section>
                    <h4>오늘은 언제인가요 ?</h4>
                    <div className="input_box">
                        <input className="input_date" type="date" value={date} onChange={(e) => setDate(e.target.value)} />
                    </div>
                </section>

                <section>
                    <h4>오늘의 감정</h4>
                    <div className="input_box emotion_list_wrapper">
                        {emotionList.map((it) => (
                            <EmotionItem key={it.emotion_id} {...it} onClick={handleClickEmotion} isSelected={it.emotion_id === emotion} />
                        ))}
                    </div>
                </section>

                <section>
                    <h4>오늘의 일기</h4>
                    <div className="input_box text_wrapper">
                        <textarea
                            placeholder="오늘은 어땟나요 ?"
                            ref={contentRef}
                            value={content}
                            onChange={(e) => setContent(e.target.value)}
                        />
                    </div>
                </section>

                <section>
                    <div className="control_box">
                        <MyButton text={'취소하기'} onClick={() => navigator(-1)} />
                        <MyButton text={'작성완료'} type={'positive'} onClick={handleSubmit} />
                    </div>
                </section>
            </div>
        </div>
    );
};

export default DiaryEditor;
```

자 한줄씩 해석해보자,

1. 네비게이터 사용을 위한 선언

```
const navigator = useNavigate();
```

2. 날짜데이터 상태, getStringDate함수는 return date.toISOString().slice(0, 10); 을 리턴하는데,
   toISOString은 2011-10-05T14:48:00.000Z 이런형식으로 Date값을 뽑아내기 때문에 slice를 사용하여 2011-10-05 만 잘라낼 수 있다.

```
const [date, setDate] = useState(getStringDate(new Date()));
```

3. 오늘의감정 상태

```
const [emotion, setEmotion] = useState(3);
```

4. textarea 상태

```
const [content, setContent] = useState();
```

5. textarea의 Ref

```
const contentRef = useRef();
```

6. App컴포넌트의 onCreate함수 사용을 위한 Context 선언

```
const { onCreate } = useContext(DiaryDispatchContext);
```

7. 감정값을 바꾸기 위한 함수 선언, 감정 리스트 부분 클릭시 아이디값을 받아와서 상태를 변화시킨다.

```
const handleClickEmotion = (emotion) => {
    setEmotion(emotion);
};
```

8. 일기작성완료를 눌렀을 때 호출하는 함수, 컨텐츠(textarea) 의 길이를 검사하고 일정길이 이상이면 onCreate를 호출하여 현재 년월일, 내용, 감정을 넘겨준다.
   그리고 navigator의 두번째 값으로 작성이 완료되면 뒤로가기가 안되게끔 replace: true값으로 지정한다.

```
const handleSubmit = () => {
    if (content.length < 1) {
        contentRef.current.focus();
        return;
    }
    onCreate(date, content, emotion);
    navigator('/', { replace: true });
};
```

9. 상단 Header선언을 위한 MyHeader컴포넌트를 호출하여 가운데 글자와 왼쪽버튼을 정의한다. 우측버튼은 없으니 생략하고 좌측버튼 이벤트는 navigator(-1)사용하여 뒤로가는 버튼으로 작성한다.

```
<MyHeader headText={'새 일기쓰기'} leftChild={<MyButton text={'< 뒤로가기'} onClick={() => navigator(-1)} />} />
```

10. input Date 타입을 사용하여 날짜를 입력받는다. 초기값은 날짜데이터 상태인 date를 넣고 onChange이벤트는 선택한 날짜로 date 상태를 변경시킨다.

```
<section>
    <h4>오늘은 언제인가요 ?</h4>
    <div className="input_box">
        <input className="input_date" type="date" value={date} onChange={(e) => setDate(e.target.value)} />
    </div>
</section>
```

11. 오늘의 감정을 받는 섹션, 감정의 대한 정보는 emotionList배열에 담겨있고 그 값을 map으로 순회하며 EmotionItem컴포넌트에 값을 전달한다.
    emotionList배열에 아이디는 key값으로 사용하고 전체 내용을 spread를 사용하여 넘겨준다. click이벤트는 갑정상태를 바꾸는 handleClickEmotion,  
    isSelected는 현재 감정의 값과 같은 개체를 찾기 위해 === 을 사용하여 true/false값을 넘겨준다. 그 후 작업은 EmotionList컴포넌트에서 이루어진다.

```
<section>
    <h4>오늘의 감정</h4>
    <div className="input_box emotion_list_wrapper">
        {emotionList.map((it) => (
            <EmotionItem key={it.emotion_id} {...it} onClick={handleClickEmotion} isSelected={it.emotion_id === emotion} />
        ))}
    </div>
</section>
```

12. textarea가 위치하는 섹션. ref는 컨텐츠 길이의 따라 focus를 위해 지정하고 value는 상태값, onChange이벤트는 content의 값을 변화시킨다.

```
<section>
    <h4>오늘의 일기</h4>
    <div className="input_box text_wrapper">
        <textarea
            placeholder="오늘은 어땟나요 ?"
            ref={contentRef}
            value={content}
            onChange={(e) => setContent(e.target.value)}
        />
    </div>
</section>
```

13. 버튼섹션. 취소하기 버튼은 -1값으로 뒤로가게 구현, 작성완료버튼은 App컴포넌트의 onCreate함수를 사용하기 위한 handleSubmit함수 호출을 한다.

```
<section>
    <div className="control_box">
        <MyButton text={'취소하기'} onClick={() => navigator(-1)} />
        <MyButton text={'작성완료'} type={'positive'} onClick={handleSubmit} />
    </div>
</section>
```

3. EmotionList

```
const EmotionItem = ({ emotion_id, emotion_img, emotion_descript, onClick, isSelected }) => {
    return (
        <div
            className={['EmotionItem', isSelected ? `EmotionItem_on_${emotion_id}` : `EmotionItem_off`].join(' ')}
            onClick={() => onClick(emotion_id)}
        >
            <img src={emotion_img} />
            <span>{emotion_descript}</span>
        </div>
    );
};

export default EmotionItem;
```

DiaryEditor 컴포넌트에서 emotion_id, emotion_img, emotion_descript와 Click이벤트 그리고 선택이 되었는지 확인하는 isSelected를 받아왔다.  
emotion_img는 이미지 호출 emotion_descript는 내용호출을 하고 isSelected여부에 따라 className을 변경하도록 배열로 선언하여 사용하였다. 그리고 클릭이벤트는 자식이벤트에서 부모에게 올라가야 함으로 onClick함수를 호출하여 아이디값을 넘겨주었다. onClick함수는

```
const handleClickEmotion = (emotion) => {
    setEmotion(emotion);
};
```

위 함수를 호출해서 감정의 값을 바꾼다.
