# React에서 사용자 입력 처리하기

react에서 다양한 사용자의 입력을 어떻게 처리할 것인지 알아보자.

1. input
2. textarea
3. select

## input

```
const Editor = () => {
    return(
        <div className="Editor">
            <h2>입력</h2>
            <div>
                <input />
            </div>
        </div>
    );
};
export default Editor;
```

input 태그는 사용자에게 한줄짜리 입력받을 수 있는 태그이다. 이 input태그를 react에서 input태그값을 직접 핸드링할 수 있도록 만들어야한다.  
사용자의 입력을 핸들링하기 위해 State를 사용할 수 있다.

```
import { useState } from 'react';
const Editor = () => {

    const [input, setInput] = useState("");
    return(
        <div className="Editor">
            <h2>입력</h2>
            <div>
                <input />
            </div>
        </div>
    );
};
export default Editor;
```

상태관리를 위해 useState를 import하고 input에 들어갈 내용인 input과 input의 상태변화를 컨트롤할 setInput까지 선언하였다.  
그럼 이제 해야할 것은 input의 내용이 바뀌면 실시간으로 입력값이 바뀔때 마다 setInput을 활용해서 input의 입력값을 저장해주면 된다.

```
import { useState } from 'react';
const Editor = () => {

    const [input, setInput] = useState("");
    return(
        <div className="Editor">
            <h2>입력</h2>
            <div>
                <input
                    name="author"
                    value={input}
                    onChange={(e) => {
                        console.log(e.target.name);
                        console.log(e.target.value);
                        setInput(e.target.value);
                    }}
                />
            </div>
        </div>
    );
};
export default Editor;
```

위 코드를 보면 input에 value={input}으로 초기값을 세팅해주었고, onChange에서 이벤트객체를 받아서 setInput을 통해 e.target.value로 값을 지정하고 있다.  
그리고 value뿐만아니라 name으로 지정한 author도 e.target.name으로 같이 출력할 수 있다.

## textarea

textarea는 여러줄 입력을 받을 수 있는 태그이다. 근데 textarea도 위 input태그와 사용방법이 완전히 똑같다.

```
import { useState } from 'react';
const Editor = () => {

    const [input, setInput] = useState("");
    const [content, setContent] = useState("");

    return(
        <div className="Editor">
            <h2>입력</h2>
            <div>
                <input
                    name="author"
                    value={input}
                    onChange={(e) => {
                        console.log(e.target.name);
                        console.log(e.target.value);
                        setInput(e.target.value);
                    }}
                />
            </div>
            <div>
                <textarea
                    name="content"
                    value={content}
                    onChange={(e) => {
                        console.log(e.target.name);
                        console.log(e.target.value);
                        setContent(e.target.value);
                    }}
                />
            </div>
        </div>
    );
};
export default Editor;
```

사용방법이 완전히 똑같다. value를 지정하고 useState를 만들어서 그 값과 set을 통해 상태를 변화시키고 있다.

## input, textarea 사용방법이 똑같은데, 하나로 할 순 없을까 ?

둘의 사용법도 완전히 똑같고 초기값도 "" 빈값으로 동일하다. 하나로 관리할 수 없을까 ? 당연히 할 수 있다.

```
import { useState } from 'react';
const Editor = () => {

    const [state, setState] = useState({
        input : "",
        content: ""
    });

    return(
        <div className="Editor">
            <h2>입력</h2>
            <div>
                <input
                    name="author"
                    value={state.input}
                    onChange={(e) => {
                        setState({
                            input: e.target.value,
                            content: state.content
                        })
                    }}
                />
            </div>
            <div>
                <textarea
                    name="content"
                    value={state.content}
                    onChange={(e) => {
                        setState({
                            content: e.target.value,
                            input: state.input
                        })
                    }}
                />
            </div>
        </div>
    );
};
export default Editor;
```

위와 같은 코드로 작성하면 동일하게 작업이된다. 순서대로 살펴보자면,

1. const [state, setState] = useState에 객체로 두개의 값을 "" 으로 전달한다. 그럼 공백으로 두 값이 초기화 된다.
2. 만약에 input에 1이라고 입력을 했다면 onChange 콜백함수가가 실행되는데 그때 당시의 e.target.value는 1이다.
3. setState는 input과 content 2개를 가지고 있는 객체이기 때문에 State를 변경시키려고 하면 새로운 객체를 만들어서 전달해줘야 한다.
4. 그럼 이떄 input은 1로 변경이 되어야 하지만 content는 값이 변경이 되면 안되고 유지해야한다.
5. 그래서 input은 e.target.value로 변경하고 content는 state.content로 그대로 "" 공백을 넣어준것이다.
6. textarea도 input과 동일한 매커니즘이다. content만 변경시키고 input은 현재 지닌 값 그대로를 넣어주었다.

여기서 한번더 업그레이드 해보자. 만약 객체안에 관리해야할 state가 10개 이상이라면 ? setState를 쓸때 아주 귀찮아 질거같다. 이럴때 spread를 사용하면 조금더 편하게 사용이 가능하다.

```
import { useState } from 'react';
const Editor = () => {

    const [state, setState] = useState({
        input : "",
        content: ""
    });

    return(
        <div className="Editor">
            <h2>입력</h2>
            <div>
                <input
                    name="author"
                    value={state.input}
                    onChange={(e) => {
                        setState({
                            ...state,
                            input: e.target.value,
                        })
                    }}
                />
            </div>
            <div>
                <textarea
                    name="content"
                    value={state.content}
                    onChange={(e) => {
                        setState({
                            ...state,
                            content: e.target.value,
                        })
                    }}
                />
            </div>
        </div>
    );
};
export default Editor;
```

spread를 사용하면 객체전체를 펼쳐주기 때문에 기존의 값을 하나하나 지정해줄 필요가없다. spread다음에 우리가 바꿀 값만 따로 지정해서 값을 지정해주면 끝난다.  
하지만 spread를 사용할때는 주의사항이 **_항상 spread가 먼저 선언되어야 한다._** 늦게 선언되게 되면 변경된 값을 다시 spread로 초기값으로 돌려버리기 때문에 값이 변경되지 않는다.

자 여기서 끝난게 아니다, 다시 코드를 보면 onchange이벤트도 하나의 함수로 합칠 수 있을 거같다. 한번 합쳐보자.

```
import { useState } from 'react';
const Editor = () => {

    const [state, setState] = useState({
        input : "",
        content: ""
    });

    const handleChangeState = (e) => {
        setState({
            ...state,
            [e.target.name]: e.target.value,
        });
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
        </div>
    );
};
export default Editor;
```

위 코드와 같이 줄일 수 있다. 순서대로 보면,

1. handleChangeState라는 함수를 만든다.
2. input과 textarea에 onChange에 handleChangeState를 호출한다.
3. 그럼 input과 textarea에 값이 변경될때 마다 handleChangeState를 호출하게 된다.
4. input과 textarea에 name을 지정한다.
5. setState에 spread로 기존값들을 지정한다.
6. 만약 input에서 1이라는 텍스트를 입력했다면 handleChangeState함수가 호출되면서 e.target.name은 author, e.target.value는 1이라는 값으로 넘어간다.
7. spread가 그대로 펼쳐지고 author: 1로 값이 변경된다.
8. 만약 content에 1이라는 텍스트를 입력하면 content: 1이라고 값이 변경될 것이다.

## select

위 코드에서 1~5까지의 점수를 나타내고 선택할 수 있는 select tag를 만들어보자.

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
        </div>
    );
};
export default Editor;
```

select, option태그를 사용하여 1~5점까지의 점수를 만들고 그 것을 input, textarea와 똑같이 value를 지정하고 handleChangeState함수를 호출했다.  
이와 같이 select도 input, textarea와 관리하는 것이 거의 동일하다. 순서대로 보자면,

1. state에 emotion이라는 프로퍼티를 추가한다. 그리고 select의 값을 저장시킬 것이기 때문에 초기값을 1로 저장하였다.
2. select 태그에 변화가 일어나면(1에서 3으로 바꿧다면) handleChangeState함수를 호출한다.
3. 그럼 e.target.name은 emotion, e.target.value는 변경된 값(3)이다.

그럼 이제 마지막으로 저장버튼을 하나 추가하여 모든 값이 어떻게 셋팅 되었는지 확인해보자.

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

마무리 코드이다. 이렇게되면 input, textarea, select의 값을 변경 후에 저장하기를 누르면 handleSubmit이 호출되면서 현재 state값을 모두 뽑아낼 수 있게된다.
