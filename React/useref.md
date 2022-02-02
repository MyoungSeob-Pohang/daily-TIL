# useRef

웹 페이지를 작성하다보면 특히 로그인이나 회원가입을 할 때 양식에 맞춰서 작성하는 경우가 많다.  
만약 사용자가 양식에 맞지않게 작성을 했다면 경고창을 띄우거나, 빨간색 박스로 바뀌면서 focus가 변경이 되는데 이 부분을 useRef를 사용하여 리액트스럽게 만들 수 있다.

[사용자 입력 처리하기](https://github.com/MyoungSeob-Pohang/daily-TIL/blob/main/React/userInput.md) 에서 사용한 코드에 이어서 사용해보자.

```
import { useState } from 'react';
const Editor = () => {

    const [state, setState] = useState({
        input : "",
        content: "",
        emotion : 1,
    });

    const handleChangeState = (e) => {
        setState({
            ...state,
            [e.target.name]: e.target.value,
        });
    };

    const handleSubmit = (e) => {
        console.log(state);
        alert('저장성공');
    }

    return(
        <div className="Editor">
            <h2>입력</h2>
            <div>
                <input
                    name="author"
                    value={state.input}
                    onChange={handleChangeState)
                />
            </div>
            <div>
                <textarea
                    name="content"
                    value={state.content}
                    onChange={handleChangeState}
                />
            </div>
            <div>
                <select
                    name="emotion"
                    value={state.emotion}
                    onChange={handleChangeState}
                >
                    <option value={1}>1</option>
                    <option value={2}>2</option>
                    <option value={3}>3</option>
                    <option value={4}>4</option>
                    <option value={5}>5</option>
                </select>
            </div>
            <div>
                <button onClick={handleSubmit}>저장하기</button>
            </div>
        </div>
    );
};
export default Editor;
```

위 코드에서 제목과 본문에 특정길이 이상의 정보가 들어왔는지 확인하고 조건에 맞다면 저장성공을, 조건에 맞지않다면 해당부분에 focus를 주는 코드로 업그레이드 해보자.
위 코드에서 수정해야 하는 부분은 저장하기 버튼을 눌렀을때 호출되는 handleSubmit 함수이다. 저장할때 조건을 확인하고 조건에 맞게 처리해야 하기 때문이다.  
순서는 아래와 같다.

useRef : getElementById, querySelector와 같이 DOM을 선택할 때 리액트는 ref라는 것을 사용한다.

1. useRef 기능을 선언한다.

```
import { useRef, useState } from 'react';
```

2. const authorInput 에 useRef()의 반환값을 담는다.

```
const authorInput = useRef();
```

3. 넣고 싶은 요소에 ref를 담아준다.

```
<input
    ref={authorInput}
    name="author"
    value={state.input}
    onChange={handleChangeState)
/>
```

4. 함수에서 포커스 이벤트를 준다.
   current : 현재 가르키는 값을 current라는 프로퍼티로 불러올 수 있다. 그곳에 focus를 주는것이다.

```
const handleSubmit = (e) => {
    if(state.input.length < 1) {
        authorInput.current.focus();
        return;
    };

    alert('저장완료');
}
```

5. 전체코드

```
import { useRef, useState } from 'react';
const Editor = () => {
    const authorInput = useRef();
    const contentInput = useRef();

    const [state, setState] = useState({
        input : "",
        content: "",
        emotion : 1,
    });

    const handleChangeState = (e) => {
        setState({
            ...state,
            [e.target.name]: e.target.value,
        });
    };

    const handleSubmit = (e) => {
        if(state.input.length < 1) {
            authorInput.current.focus();
            return;
        };

        if(state.content.length < 1) {
            contentInput.current.focus();
            return;
        };

        alert('저장완료');
    }

    return(
        <div className="Editor">
            <h2>입력</h2>
            <div>
                <input
                    ref={authorInput}
                    name="author"
                    value={state.input}
                    onChange={handleChangeState)
                />
            </div>
            <div>
                <textarea
                    ref={contentInput}
                    name="content"
                    value={state.content}
                    onChange={handleChangeState}
                />
            </div>
            <div>
                <select
                    name="emotion"
                    value={state.emotion}
                    onChange={handleChangeState}
                >
                    <option value={1}>1</option>
                    <option value={2}>2</option>
                    <option value={3}>3</option>
                    <option value={4}>4</option>
                    <option value={5}>5</option>
                </select>
            </div>
            <div>
                <button onClick={handleSubmit}>저장하기</button>
            </div>
        </div>
    );
};
export default Editor;
```
