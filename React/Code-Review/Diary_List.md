# Diary_List

[HOME](https://github.com/MyoungSeob-Pohang/daily-TIL/blob/main/Code-Review/Diary_Home_List.md) 에서 이어지는 문서

화면 상단부를 만들었으니 이제 아래에 일기리스트와 그에 맞는 감정이미지 그리고 수정하기 버튼까지 구현해보자.  
일단 일기리스트가 나오기 전 정렬하는 select가 2개가 존재한다. 하나는 최신, 오래된순 정렬과 좋은감정을 가진일기, 나쁜 감정을 가진일기를 각각 정렬하는 부분부터 구현하자 .

## 최신순 , 오래된 순 정렬

일단 컴포넌트로 분리하여 ControlMenu를 만들어 준다. 이 ControlMenu가 select를 구성하고 그것을 리턴하고 있으며 가져오는 값은 value : 처음 가지는 select의 값 onChange : 체인지 이벤트 optionList: 객체로 된 정렬 옵션 이다. 하나하나 설명하겠다.

```
const ControlMenu = ({ value, onChange, optionList }) => {
    return (
        <select className="ControlMenu" value={value} onChange={(e) => onChange(e.target.value)}>
            {optionList.map((it, idx) => (
                <option key={idx} value={it.value}>
                    {it.name}
                </option>
            ))}
        </select>
    );
};
```

1. DiaryList에서 일단 정렬을 위한 상태를 선언한다. 초기값은 최신을 뜻하는 latest로 정해주었다.

```
const [sortType, setSortType] = useState('latest');
```

2. option구성을 위한 옵션 리스트 선언. 최신순과 오래된 순이 option 값으로 들어갈 것이고 그 option의 value로 latest, oldest로 지정해주었다.

```
const sortOptionList = [
    { value: 'latest', name: '최신순' },
    { value: 'oldest', name: '오래된 순' },
];
```

3. ControlMenu 호출

```
<ControlMenu value={sortType} onChange={setSortType} optionList={sortOptionList} />
```

value는 현재 정렬 상태값을 가진 sortType, change이벤트는 정렬 상태를 수정하는 setSortType 그리고 옵션 리스트로는 sortOptionList 넘겨주었다.

```
const ControlMenu = ({ value, onChange, optionList }) => {
    return (
        <select className="ControlMenu" value={value} onChange={(e) => onChange(e.target.value)}>
            {optionList.map((it, idx) => (
                <option key={idx} value={it.value}>
                    {it.name}
                </option>
            ))}
        </select>
    );
};
```

자 그럼 다시 보면 select의 value가 value로 들어가고 onChange는 setSortType을 받으니 호출하여 현재 e.target.value를 넘겨주면 된다.  
그리고 옵션리스트를 map으로 순회하면서 출력하여 주면 된다. idx는 key값 지정을 위해 같이 넣어준다.  
이렇게 되면 최신순 , 오래된 순 select가 완성된다.

두번째 정렬인 감정에 따른 일기의 종류 select도 동일하게 만들면 된다.

1. 감정 상태 값 , 초기값은 전체 출력을 위한 all

```
const [filter, setFilter] = useState('all');
```

2. 옵션리스트 정의

```
const filterOptionList = [
    { value: 'all', name: '전부' },
    { value: 'good', name: '좋은감정만' },
    { value: 'bad', name: '나쁜감정만' },
];
```

3. ControlMenu 호출

```
<ControlMenu value={filter} onChange={setFilter} optionList={filterOptionList} />
```

## DiaryItem

자 select를 만들었으니 이제 아이템을 보여주고 그 아이템의 정렬을 select의 선택에 따라 수정해보자.  
컴포넌트 분리를 위해서 DiaryItem이라는 컴포넌트로 따로 분리하자.  
DiaryItem 컴포넌트는 전체 일기리스트를 가지고 그것을 보여줘야 하기때문에 data의 모든값을 받아와야한다.

```
import { useNavigate } from 'react-router-dom';
import MyButton from './MyButton';

const DiaryItem = ({ id, emotion, content, date }) => {
    const strDate = new Date(parseInt(date)).toLocaleDateString();
    const navigate = useNavigate();

    const goDetail = () => {
        navigate(`/diary/${id}`);
    };

    const goEdit = () => {
        navigate(`/edit/${id}`);
    };

    return (
        <div className="DiaryItem">

            <div onClick={goDetail} className={['emotion_img_wrapper', `emotion_img_wrapper_${emotion}`].join(' ')}>
                <img src={process.env.PUBLIC_URL + `assets/emotion${emotion}.png`} />
            </div>

            <div className="info_wrapper" onClick={goDetail}>
                <div className="diary_date">{strDate}</div>
                <div className="diary_content_preview">{content.slice(0, 25)}</div>
            </div>

            <div className="btn_wrapper">
                <MyButton text={'수정하기'} onClick={goEdit} />
            </div>

        </div>
    );
};

export default DiaryItem;
```

DiaryItem의 3개 div중 첫번째는 감정에 따른 이미지를 보여줘야하기 떄문에 emotion값을 변수로 사용하여 해당하는 이미지를 호출한다.
두번쨰 div는 작성한 날짜와 내용을 보여주고 있고 세번째는 수정하기로 이동하기 위한 버튼이다.

## getProcessedDiaryList

```
const getProcessedDiaryList = () => {

    const filterCallBack = (item) => {
        if (filter === 'good') {
            return parseInt(item.emotion) <= 3;
        }
        return parseInt(item.emotion) > 3;
    };

    const compare = (a, b) => {
        if (sortType === 'latest') {
            return parseInt(b.date) - parseInt(a.date);
        } else {
            return parseInt(a.date) - parseInt(b.date);
        }
    };

    const copyList = JSON.parse(JSON.stringify(diaryList));
    const filteredList = filter === 'all' ? copyList : copyList.filter((it) => filterCallBack(it));

    const sortedList = filteredList.sort(compare);
    return sortedList;
};
```

실질적 정렬을 위한 코드이다 . 하나하나 해석해보자.

1. diaryList를 그대로 조작할 수 없으니 그대로 copy해온다. JSON.stringify를 사용하면 string형으로 변환되고 그것을 다시 JSON.parse를 사용하여 JSON형식으로 변경하였다.

```
const copyList = JSON.parse(JSON.stringify(diaryList));
```

2. 감정 상태를 가진 filter를 가지고와서 하나씩 순회하면서 all 값을 가지고 있으면 copyList를 그대로 넘겨주고 아니면 필터링을 걸어서 filterCallBack을 호출한다.

```
const filteredList = filter === 'all' ? copyList : copyList.filter((it) => filterCallBack(it));
```

3. filterCallBack함수에서 아이템을 받아와 어떤 상태인지 확인한다. good이면 emotion이 3 이하 인 값 아니면 3초과 값을 리턴한다.

```
const filterCallBack = (item) => {
    if (filter === 'good') {
        return parseInt(item.emotion) <= 3;
    }

    return parseInt(item.emotion) > 3;
};
```

4. filteredList를 sorting 한다. 하지만 sort는 사전순 정렬을 들어가기 때문에 compare함수를 같이 넘겨준다.

```
const sortedList = filteredList.sort(compare);
```

5. compare

```
const compare = (a, b) => {
    if (sortType === 'latest') {
        return parseInt(b.date) - parseInt(a.date);
    } else {
        return parseInt(a.date) - parseInt(b.date);
    }
};
```

6. 정렬된 리스트 리턴

```
return sortedList;
```

7. DiaryList에서는 getProcessedDiaryList()를 호출하여 map으로 순회하면서 각 일기를 DiaryItem컴포넌트에 전달해준다.

```
{getProcessedDiaryList().map((it) => (
    <DiaryItem key={it.id} {...it} />
))}
```
