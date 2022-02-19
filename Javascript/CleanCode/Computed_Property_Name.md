# Computed Property Name

Computed Property Name이란 무엇일까, 검색해서 찾아보니 표현식을 이용해 객체의 key값을 정의하는 문법이라고 나온다. 말로 설명하면 어려워 코드를 보니 한눈에 무엇인지 알게 되었다. 리엑트를 사용하면서 가끔 사용했던 문법인 것이다.

```
const key = 'name';
const obj = {
    [key] : 'moonsbi',
}
```

위에서 대괄호[] 에 감싸진 key가 바로 Computed Property Name이였던 것이다. 리액트에서는 [e.target.name] 이런식으로 사용을 하는 것과 동일하다.

```
const handleChange = (e) => {
    setState({
        [e.target.name] : e.target.value,
    });
};

<input value={state.id} onChange={handleChange} name="name">
```

위 같은 코드를 해석해보면 input이 바뀔때마다 handleChange함수가 호출되고 handleChange는 그 이벤트값을 받아서 target에 접근하고 있는 것이다. e.target.name은 그 input에 name값인 name을 받아와서 state.id를 할당해주는 것이다.
이렇게 Computed Property Name을 사용하면 key 값에도 변수뿐 아니라 함수도 넣어줄 수 있게 되고 템플릿 리터럴인 백틱`` 과 같이 사용하면 조금 더 유연하고 멋진코드를 짤 수 있을 것이다.
