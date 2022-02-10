# LocalStorage

지금은 더미데이터 일기를 제외 한 새로 작성 된 일기들은 새로고침하면 사라진다. JavaScript로 새로운 일기를 작성해도 JavaScript는 결국 웹브라우저에서만 동작하는 코드이기 때문에 새로고침을 하면 모두 지우고 새로 받게 된다. 그럼 작성 된 일기들이 더미데이터를 제외한 모든게 사라지는 것이다. 이런 데이터는 실제로 DB에 저장을 하여 새로고침 시 DB를 READ하여 값을 가지고와서 보여준다. 하지만 지금은 DB가 없기 때문에 Storage라는 것을 활용하여 일기데이터를 저장해보려고 한다.
로컬스토리지와 세션스토리지로 나뉘고 세션스토리지는 새로고침하거나 탭을 닫으면 모두 지워지는 휘발성이고 로컬스토리지는 코드로 삭제하거나 캐시를 삭제하지 않는 이상 절대 지워지지 않는 값이다. 즉 스토리지는 쉽게 말하면 웹브라우저에 기억시킬 수 있는 공간이고 세션스토리지 용량은 약 5mb까지, 로컬은 브라우저 마다 다르지만 굉장히 용량이 크다. 스토리지는 객체를 사용하는거 처럼 Key : Value 값을 사용하여 저장시킨다.

사용법은 간단하다. 저장은 setItem, 가져오는 것은 getItem 이다.

1. 저장

```
localStorage.setItem(key, value);
```

2. 가져오기

```
const localData = localStorage.getItem(key);
```

그럼 더미데이터를 지우고 스토리지의 값으로 일기를 만들어보자. 우선 만들기 전에 다이어리 삭제를 구현하지 않아 먼저 구현하자.

## 삭제 구현 (DiaryEditor 컴포넌트)

1. rightChild로 isEdit가 true 즉 수정 중일때 삭제버튼이 나타나게 하였다.

```
<MyHeader
    headText={isEdit ? '일기 수정하기' : '새 일기쓰기'}
    leftChild={<MyButton text={'< 뒤로가기'} onClick={() => navigator(-1)} />}
    rightChild={isEdit && <MyButton text={'삭제하기'} type={'negative'} onClick={handleRemove} />}
/>
```

2. handleRemove 함수구현, 삭제 하겠는지 물어본 후 onRemove함수를 호출하여, DiaryEditor이 Edit 컴포넌트에서 받아오는 originData의 아이디 값을 같이 넘겨주어서 해당 아이디의 데이터를 삭제하도록 한다.

```
const handleRemove = () => {
    if (window.confirm('정말 삭제 하시겠습니까 ?')) {
        onRemove(originData.id);
        navigator('/', { replace: true });
    }
};
```

3. App 컴포넌트의 reducer에는 REMOVE가 아래와 같이 정의되어 있다.

```
const reducer = (state, action) => {
    // 새로운 상태 담을 배열
    let newState = [];

    switch (action.type) {
        case 'INIT': {
            return action.data;
        }
        // data: { id: dataId.current, date: new Date(date).getTime(), content, emotion } 을 받아오니, 새로운 상태에 그대로 담고 나머진 state그대로
        case 'CREATE': {
            newState = [action.data, ...state];
            break;
        }
        // targetId를 받아와서 해당하는 아이디가 아닌 것만 모두 골라서 새로운 상태에 저장
        case 'REMOVE': {
            newState = state.filter((it) => it.id !== action.targetId);
            break;
        }
        // data: { id: targetId, date: new Date(date).getTime(), content, emotion } 같은 아이디를 가졌으면 업데이트 시키고 아니면 그대로 반환
        case 'EDIT': {
            newState = state.map((it) => (it.id === action.data.id ? { ...action.data } : it));
            break;
        }
        default:
            return state;
    }
    // switch 끝나고 새로운 상태 리턴
    return newState;
};
```

## 스토리지 활용하기

1. reducer에 마지막에 로컬스토리지 아이템을 저장 시키도록 한다. 배열은 [object, object]값으로 저장되기에 JSON.stringify로 형변환을 해준 후 value값으로 사용한다.

```
localStorage.setItem('diary', JSON.stringify(newState));
```

2. App 컴포넌트가 Mount 될 때 스토리지 아이템을 가지고 와서 초기값으로 사용한다.  
   JSON.parse로 복원 시킨 데이터를 정렬(sort)하여 diaryList에 담고 아이디 값을 지정 해주기 위해 내림차순 정렬 된 일기리스트의 제일 첫번째 요소의 id (제일 높은 id를 가진 일기) 를 가지고와서 +1 한값을 아이디로 지정한다. 그리고 useRef를 다시 0으로 초기값을 지정한다.
   이렇게 한 이유는 이미 일기리스트가 있을 경우 아이디값이 그 갯수만큼 만들어져 있을 것이다. 그래서 새로운 일기를 저장 시킬때는 가장 높은 아이디값을 가진 일기의 다음 일기이니 +1을 해줘야 하는 것이다.  
   그리고 dispatch를 호출하여 INIT으로 type를 지정하고 data는 diaryList를 준다.

```
useEffect(() => {
    const localData = localStorage.getItem('diary');

    if (localData) {
        const diaryList = JSON.parse(localData).sort((a, b) => parseInt(b.id) - parseInt(a.id));
        dataId.current = parseInt(diaryList[0].id) + 1;

        dispatch({ type: 'INIT', data: diaryList });
    }
}, []);
```
