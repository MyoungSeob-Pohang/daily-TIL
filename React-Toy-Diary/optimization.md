# 최적화 완료

## 코드

[React-Diary](https://github.com/MyoungSeob-Pohang/React-Diary)

프로젝트를 전반적으로 모두 최적화 시키도록 하자. 아직 심각한 문제가 하나 남았는데, DiaryItem을 수정/삭제하면 해당하는 아이템만 리렌더 되는게 아니라 모든 아이템이 리렌더링 된다.  
지금은 20개로 제한하여 큰 어려움이 없지만 만약 사진이나 동영상을 포함하고 있고 아이템이 100개가 넘어간다면 아주 많이 리렌더가 일어나면서 메모리낭비가 될 것이다.  
DiaryItem컴포넌트에 가서 최적화를 시켜보자. 일단 export 부분을 React.memo로 묶어 주었다.

```
export default React.memo(DiaryItem);
```

그리고 어떤 아이템이 렌더링 되는지 보기위해 useEffect를 사용하여 출력해 보았다.

```
useEffect(() => {
    console.log(`${id}번 째 아이템 렌더`);
});
```

예상되로 0번 부터 19번까지 렌더되고 있다. 그리고 수정/삭제하여도 모두 다시 렌더링이 된다. 그리고 DiaryItem은 이렇게 memo로 감싼다고하여 최적화가 되는 컴포넌트가 아니다.  
왜냐하면 onEdit, onRemove함수가 data State가 변화하면 재생성되는 함수들이기 때문이다.  
그래서 App 컴포넌트로 가서 onCreate를 최적화 한거처럼 onEdit, onRemove도 최적화 해주어야 한다.

### 기존코드

```
// 삭제 기능
const onRemove = (targetId) => {
    const newDiaryList = data.filter((it) => it.id !== targetId);
    setData(newDiaryList);
};

// 수정기능
const onEdit = (targetId, newContent) => {
    setData(data.map((it) => (it.id === targetId ? { ...it, content: newContent } : it)));
};
```

### 수정코드

```
 // 삭제 기능
const onRemove = useCallback((targetId) => {
    setData((data) => data.filter((it) => it.id !== targetId));
}, []);

// 수정기능
const onEdit = useCallback((targetId, newContent) => {
    setData((data) => data.map((it) => (it.id === targetId ? { ...it, content: newContent } : it)));
}, []);
```

useCallback으로 묶어준 뒤 onCreate를 최적화 한 것처럼 setData에 함수를 전달하고 data값을 주어서 최신 data값을 가지게 하였다.  
이렇게 하면 아이템의 값을 수정하거나 삭제하여도 해당 아이템만 렌더링이 되고 나머지 아이템들은 렌더링이 되지 않는 최적화가 완성되었다.
