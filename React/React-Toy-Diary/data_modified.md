# 데이터 수정하기

이번에는 등록 된 일기를 수정하는 기능을 만들어보자.

일단은 수정 기능을 만들기 위해서 DiaryItem.js 컴포넌트로 이동하여 삭제하기 버튼 옆에 수정하기 버튼을 하나 만들어주었다.  
그리고 삭제하기 버튼에 onClick 코드가 좀 길어서 따로 handleRemove()함수로 따로 빼주었다.

그럼 첫번째로 수정하기 버튼을 누르면 어떻게 되야 할지 생각해보자,  
수정하기 버튼을 누르면 일단 내용이 나오는게 아니라 수정 할 수 있는 textarea로 바뀌어야 할거같다.  
이것을 state로 해볼것인데 이 상태를 저장할 const [isEdit, setIsEdit] = useState(false); 을 만들었다.  
수정 중일때는 true로 바꿔줄 것이고 수정 중이 아닐때는 false가 될것이다. 즉 수정하고 있는 중인지 수정 중이 아닌지 확인할 값이다.  
만약 isEdit 이 true가 되면 render 되는 코드를 수정 중으로 간주해서 코드를 작성하면 되고 false이면 기존에 값 그대로 사용하면 된다.

그리고 const toggleIsEdit = () => setIsEdit(!isEdit); 이라는 코드를 선언하여서 toggleEdit함수가 호출되면 setIsEdit에 not연산자를 사용하여 원래 isEdit이 가진 값을 반전 시켜준다.  
하지만 아직 isEdit에 따라 어떤 작업을 하여야 하는지 코드가 없어서 아무일이 일어나지 않는다. isEdit에 따라서 컨텐츠 부분이 그대로 나올지 수정폼이 나올지 정의해보자.

```
<div className="content">
    {isEdit ? (
            <>
                <textarea></textarea>
            </>
        ) : (
            <>
                {content}
            </>
    )}
</div>
```

지금보면 content div안에 삼항연산을 사용하여 표현하였다. isEdit이 true 값이면 textarea(수정폼)이 나오고 아니면 content값을 그대로 보여주어라 라는 코드이다.
이제 수정버튼을 누르면 textarea가 잘 나온다 , 하지만 이 textarea input값도 react에서 상태로 관리해 주어야 한다.

```
const [localContent, setLocalContent] = useState("");
```

그리고 이 값을 textarea에 연결시켜주자.

```
<textarea value={localContent} onChange={(e) => setLocalContent(e.target.value)}></textarea>
```

자 이번에는 수정하기 버튼을 눌렀을 때 아래의 버튼도 바뀌어야 한다. 아까 작성한 textarea를 나타내는 삼항연산자를 동일하게 사용하여 버튼도 바꿔보도록 하자.

```
{isEdit ? (
        <>
	        <button onClick={handleQuitEdit}>취소</button>
            <button onClick={handleEdit}>완료</button>
        </>
    ) : (
        <>
            <button onClick={handleRemove}>삭제</button>
            <button onClick={toggleIsEdit}>수정</button>
        </>
)}
```

isEdit의 값에 따라서 isEdit이 true(수정 중)면 완료, 취소버튼으로 false(수정 중 아님)면 삭제, 수정으로 나오게 끔 코드를 작성하였다.

자 다음 문제는 수정하기 버튼을 눌러 textarea가 나오면 빈값으로 나온다, 보통 수정하기 버튼을 누르게 되면 작성된 내용이 그대로 나오고 그 내용을 수정하여야 하는데 아무것도 없는 빈값이 나와버리니, 이 부분을 수정해보도록 하자 . 이 부분은 굉장히 쉽다. 아까 textarea의 상태를 관리하기 위한 State에 기본 값으로 빈 값이 아닌

```
const [localContent, setLocalContent] = useState(content);
```

콘텐츠의 값을 넣어버리면 된다. 그럼 기본 값이 content가 되고 이것을 textarea에 value에 넣었으니 기존의 값이 그대로 들어오게 된다.
하지만 여기서 또 문제가 발생한다 . 수정하기 버튼을 누르고 textarea에 글자를 막 적은 후에 취소를 누른다음 다시 수정하기를 하면 아까 적은 글들이 그대로 남아있다.
이 이유는 지금 textarea의 값의 상태는 content가 아니라 localContent가 관리하기 때문인데 수정하기를 누르고 값을 입력후에 수정취소를 누른다고 해도 값이 그대로 남아있다. 이 초기화를 위한 함수를 선언해보자.

```
const handleQuitEdit = () => {
    setIsEdit(false);
    setLocalContent(content);
};
```

취소하기를 누른다는 것은 수정하기를 종료한다는 뜻이니 setIsEdit(false); 값으로 바꿔주고 setLocalContent(content); 를 통해서 다시 초기 content값을 넣어주면 된다.
그리고 이 함수를 취소버튼에 연결해주면 된다.

자 그럼 이제 마지막인 수정 후 완료를 눌러 정말로 수정되도록 하는 것이다.  
하지만 이전 문서에서 말했듯 리액트 특성상 데이터는 위에서 아래로, 이벤트는 아래에서 위로 올라간다고 말했는데 이 수정완료 이벤트를 DiaryItem에서 App까지 전달을 해야한다.  
결국 App에서 수정하는 기능을 만들어서 App에서 DiaryItem까지 내려줘야 한다. 그럼 App컴포넌트에서 수정하는 기능을 만들어보자.

```
const onEdit = (targetId, newContent) => {
    setData(data.map((it) => (it.id === targetId ? { ...it, content: newContent } : it)));
};
```

onEdit함수는 id값과 수정된 내용을 받아서(targetId, newContent) setData함수를 호출해서 map함수로 같은 아이디를 가진 객체가 있으면 원본데이터를 가지고 오고 content를 newContent로 수정을 하고 아니면 수정대상이 아님으로 it을 그대로 내보내면 된다.  
그리고 이 함수를 DiaryList컴포넌트로 내려주고

```
<DiaryList onRemove={onRemove} diaryList={data} onEdit={onEdit} />
```

DiaryList컴포넌트에서도 const DiaryList = ({ onRemove, diaryList, onEdit }) props로 받아 자식요소인

```
<DiaryItem key={it.id} {...it} onRemove={onRemove} onEdit={onEdit} />
```

다시 넘겨준다.  
그럼 DiaryItem에서 const DiaryItem = ({ onEdit, onRemove, author, content, emotion, create_date, id }) props로 받아서 사용이 가능하다.

그럼 이제 DiaryItem컴포넌트에서 함수를 선언해서 onEdit을 사용해보자.

```
const handleEdit = () => {
    if (localContent.length < 5) {
        localContentInput.current.focus();
        return;
    }
    if (window.confirm(`${id}번째 일기를 수정하시겠습니까 ?`)) {
        onEdit(id, localContent);
        toggleIsEdit();
    }
};
```

focus보내는 부분은 생략하고 설명을 하자면 window.confirm으로 true값이 반환되면 onEdit을 호출 후에 id와 localContent를 그대로 넘겨주게 된다.  
localContent는 textarea에서 수정된 값을 가지고 있을 것이고 이 값이 App 컴포넌트의 onEdit을 호출하여 값이 변경된 값을 리턴받게 된다.  
값이 변경되고 나면 수정이 끝낫기 때문에 toggleIsEdit(); 을 호출하여 false로 변환하여 준다.
