# Optimization(최적화)

이제 마무리 작업으로 최적화 작업에 들어가자. 우선 최적화전에 알아야할 것은 어떤 부분에서 연산이 낭비되고 있는가 ? 이다. 지금 작성된 코드들이 정상적으로 최적화 되어있는지 확인하는 방법은 2가지가 있는데, 첫번째는 코드를 하나하나 보면서 판단하는 것과, 두번째는 도구의 힘을 빌리는 것인데 리액트에서 이것을 도와줄 도구는 바로 React-Developer-Tools이다. 크롬 확장프로그램으로 다운로드 받으면 된다.  
React-Developer-Tools는 컴포넌트의 state를 변화시켰을 때 어떤 컴포넌트가 리렌더 되는지 알 수 있다.

하나하나 찾아보자.

1. 날짜를 바꾸면 아래 필터와 새 일기쓰기 버튼이 리렌더 된다. 날짜를 바꾼다고 아래 필터까지 리렌더 될 필요는 없다. 연산낭비 임으로 수정하도록 하자.
   지금 날짜를 바꾸는 버튼은 Home컴포넌트의 leftChild와 rightChild에 전달된 MyButton에 decreaseMonth함수와 increaseMonth가 각각 실행한다.
   구조를 보면 Home컴포넌트 자식으로 DiaryList컴포넌트가 있고 DiaryList컴포넌트의 자식으로 이 필터들이 존재하는 ControlMenu컴포넌트가 있다. 즉 Home이 리렌더가 일어나면 자식인 DiaryList도 리렌더가 일어나고 또한 그의 자식인 DiaryList도 리렌더가 일어난다. 그래서 ControlMenu컴포넌트는 리렌더가 되지않도록 React.memo를 사용하여 리렌더를 막도록 하자.

```
// Header
<MyHeader
    headText={headText}
    leftChild={<MyButton text={'<'} onClick={decreaseMonth} />}
    rightChild={<MyButton text={'>'} onClick={increaseMonth} />}
/>
```

```
// 달 증가 함수
const increaseMonth = () => {
    setCurDate(new Date(curDate.getFullYear(), curDate.getMonth() + 1, curDate.getDate()));
};

// 달 감소 함수
const decreaseMonth = () => {
    setCurDate(new Date(curDate.getFullYear(), curDate.getMonth() - 1, curDate.getDate()));
};
```

```
// select 컨트롤 메뉴
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

위 ControlMenu에 React.memo를 사용하자.

```
const ControlMenu = React.memo(({ value, onChange, optionList }) => {
    return (
        <select className="ControlMenu" value={value} onChange={(e) => onChange(e.target.value)}>
            {optionList.map((it, idx) => (
                <option key={idx} value={it.value}>
                    {it.name}
                </option>
            ))}
        </select>
    );
});
```

React.memo를 사용하게 되면 강화된 컴포넌트가 반환되는데 결국은 리렌더 조건이 걸린것이다. 전달받는 props인 value, onChange, optionList가 변하지않으면 리렌더 되지 않는다. value, onChange, optionList가 변할때만 리렌더가 되는 것이다.

근데 이상한 점이 하나 있다.  
이전코드에서 onChange함수는 useCallback 같은 기능으로 재사용하게 만들지 않으면 부모가 리렌더 되면서 변경이 되어 React.memo가 정상적으로 작동이 되지 않았었는데,
그때 전달 된 onChange의 값들은 set으로 시작되는 상태 변화함수가 아니였다.

```
// 정렬을 위한 State 최신순/오래된 순 초기 값은 최신 순
const [sortType, setSortType] = useState('latest');

// 감정 State 초기값은 전체 표시를 위해 all
const [filter, setFilter] = useState('all');

{/* 최신순 / 오래된 순 정렬 */}
<ControlMenu value={sortType} onChange={setSortType} optionList={sortOptionList} />

{/* 감정에 따른 정렬 */}
<ControlMenu value={filter} onChange={setFilter} optionList={filterOptionList} />
```

지금 보면 setSortType, setFilter는 상태를 변화시킬 수 있는 함수이다. 그런데 만약에 상태변화를 시키고 있는 구현 된 함수를 props로 전달하게 되면 React.memo를 작성해 두 었지만 날짜를 바꿀 때 마다 계속 리렌더가 될 것이다. 왜냐하면 상태변화를 시키고 있는 구현 된 함수는 컴포넌트가 리렌더될 때 다시 재생성이 될것이다. 그래서 React.memo에서 비교를 할때 서로 다른 prop이라고 비교를 하기 때문이다. 값은 같은데 재생성이 되어서 결국 아이디 값이 달라진다고 이해하면 된다.

하지만 setSortType, setFilter 같이 상태 변화함수는 동일한 아이디 값이 전달된다. 그래서 이 함수는 기본적으로 useCallback 처리가 되어있다고 생각하면 된다. 그래서 굳이 함수를 만들어 상태를 변화시킬 필요가 없는 경우에는 상태변화함수 그 자체를 내려주는게 좋다.

2. 필터를 적용하면 위치만 바뀌면 되는데 안에 일기리스트 모두가 리렌더 되는 부분

자 DiaryItem컴포넌트에서 일기리스트를 만들어주는데 DiaryItem는 DiaryList컴포넌트의 자식이다. 즉 필터를 가진 DiaryList컴포넌트가 필터값이 바뀔 때 리렌더가 되는데 그러니 자동적으로 DiaryItem컴포넌트도 리렌더가 되면서 일기목록을 다시가지고 온다. 일기목록을 다시 가지고 오는것은 굉장히 부담이 크다. 목록자체가 얼마없을 때는 괜찮지만 양이 많아지거나 이미지, 동영상을 포함한 애들이라면 필터를 적용시킬 때 마다 렉이 걸릴 것이다. 그래서 DiaryItem컴포넌트도 React.memo로 묶어 주었다.

```
export default React.memo(DiaryItem);
```

3. 수정하기 페이지에서 일기 content수정 시 감정부분 리렌더

이 부분도 동일하다. DiaryEditor컴포넌트에서 EmotionItem컴포넌트를 자식으로 두고 있는데, 일기 content를 수정하게 되면 당연히 DiaryEditor컴포넌트가 리렌더가 일어난다. 그래서 자식인 EmotionItem컴포넌트도 당연히 리렌더가 일어나는 것이다. 자 그럼 EmotionItem컴포넌트도 React.memo로 묶어주자.

```
export default React.memo(EmotionItem);
```

하지만 확인해 보니 React.memo를 사용했음에도 계속 리렌더가 되고 있다. 왜그럴까 ? 바로 전달받는 prop중에

```
const EmotionItem = ({ emotion_id, emotion_img, emotion_descript, onClick, isSelected }) => {
```

이 onClick 때문이다. 부모인 DiaryEditor컴포넌트에서는 EmotionItem컴포넌트를 아래와 같이 호출하고 있다.

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

이 onClick 이벤트에 전달되는 handleClickEmotion는 아래와 같다.

```
const handleClickEmotion = (emotion) => {
    setEmotion(emotion);
};
```

위에서 얘기한 것과 동일하다. 상태변화함수를 일으키는 작업을 하는 함수를 onClick이벤트로 넘겨주었기 때문에 리렌더가 되면 아이디 값이 달라진다 하였다.  
이 때는 EmotionItem컴포넌트에 setEmotion을 그대로 전달하거나 useCallback을 사용하면 된다.

```
const handleClickEmotion = useCallback((emotion) => {
    setEmotion(emotion);
}, []);
```

이렇게 되면 마운트 되는시점에 한번 만들어지고 계속 재사용을 하기때문에 크게 문제가 생기지 않는다.
