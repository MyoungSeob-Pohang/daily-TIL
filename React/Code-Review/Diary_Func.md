# 상세 Diary 코드 상세 리뷰 < 기능 정의 >

[상태관리](https://github.com/MyoungSeob-Pohang/daily-TIL/blob/main/Code-Review/Diary_State.md) 에서 다루었던 전체코드 중 기능정의한 함수에 대해 설명

이 기능들은 App인 부모컴포넌트에서 정의하여 자식컴포넌트들에게 보내줄 함수이다. 전체 Data는 App인 부모가 가지고 있고, 그 Data를 조작하는 것은 자식컴포넌트이다.

1. CREATE (새로운 일기 작성)

```
const onCreate = (date, content, emotion) => {
    dispatch({ type: 'CREATE', data: { id: dataId.current, date: new Date(date).getTime(), content, emotion } });
    dataId.current += 1;
};
```

1. date : 새롭게 작성 될 일기의 시간
2. content : 새롭게 작성 될 일기의 내용
3. emotion : 새롭게 작성 될 일기의 감정점수

이 3가지를 전달 받아 dispatch를 통해 data를 전달하고 있다. 그럼 reducer함수가 받아서 아래코드를 실행한다.

```
case 'CREATE': {
    newState = [action.data, ...state];
    break;
}
```

새롭게 작성 될 일기와 기존 일기를 담아서 newState저장 시키고 그것을 리턴한다. 그럼 결국 전체일기에서 새로운 일기가 추가되는 것과 동일하다.  
그리고 id로 사용할 useRef의 값을 1 증가시켜 준다.

2. REMOVE (일기 삭제)

```
const onRemove = (targetId) => {
    dispatch({ type: 'REMOVE', targetId });
};
```

1. 해당 일기의 id값을 받아서 dispatch에 전달한다. 그럼 reducer함수가 받아서 아래코드를 실행한다.

```
case 'REMOVE': {
    newState = state.filter((it) => it.id !== action.targetId);
    break;
}
```

전체 일기를 가진 state에 필터링을 걸어 하나씩 순회하면서 받아온 id값과 일치하지 않는 모든 요소를 반환한다. 즉 받아온 id빼고 다른 일기들을 newState에 저장시키는 것
그리고 그걸 리턴한다. 그럼 전체 일기에서 삭제누른 일기를 제외한 일기들만 살아남는다.

3. EDIT (일기 수정)

```
const onEdit = (targetId, date, content, emotion) => {
    dispatch({ type: 'EDIT', data: { id: targetId, date: new Date(date).getTime(), content, emotion } });
};
```

1. 수정할 일기의 targetId, 날짜, 내용, 감정점수를 모두 받아서 dispatch에 전달

```
case 'EDIT': {
    newState = state.map((it) => (it.id === action.data.id ? { ...action.data } : it));
    break;
}
```

2. 위 코드 실행. map을 통해서 모든 요소를 순회하면서 그 요소의 아이디 값과 받아온 아이디를 비교해 같으면 받아온 값으로 아니면 그대로 newState에 저장
3. newState 리턴

그럼 결혼적으로 내가 수정을 누른 일기의 값만 바뀌고(수정) 나머지는 그대로 반환된다.
