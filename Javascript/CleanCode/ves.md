# 값식문

문법은 중요한가? 중요하다면 왜 그럴까? 개발자는 결국 프로그래밍 언어를 사용하는 사람이다.  
개발자는 언어를 통해 컴퓨터를 이해시켜야 한다. 컴퓨터는 사람과 다르게 단 하나의 오타만 있어도 에러가 나고, 에러가 있다면 컴퓨터가 요청한 명령을 처리하지 못한다.  
그렇기 떄문에 사소한 문법 하나하나가 아주 중요한 것이다. 하지만 사실상 문법을 지킨다 라는게 말처럼 쉽지않다. 많은 개발자가 많이 헷갈리고 무시하고 넘어가는 부분이 바로 값식문이다.

1. 값 Value : 값은 조작할 수 있는 어떠한 표현을 의미한다. 값은 문자열, 문자, 숫자 처럼 어떤 데이터 형식도 가질 수 있다.
2. 식 expression : 식은 값을 결정하기 위해 평가될 수 있는 구문이다. 식은 상수, 변수, 함수, 연산자들의 조합으로 이루어진다.
3. 문 statement : 수행할 행동의 구문 단위이다. 데이터에 어떠한 행동을 취할 것인지 결정한다. 선언문, 할당문, 함수호출, 반복문, 이프(조건)문 등이 해당된다.

## 예시

React를 사용할 때 여러가지 문법적 오류가 많이 발생하는데, 아래는 React를 사용해봤다면 한번쯤은 사용해봤을 기본적인 코드이다.

```
ReactDOM.render(
    <div id="msg">Hello World</div>,
    mountNode
);
```

ReactDOM.render메서드를 호출하고 있다. 호출은 () 괄호를 의미하는데 ()소괄호 {}중괄호 []대괄호의 쓰임이 다 다르다.
지금 보면 함수에 인자를 2개 넘기고 있는데, 하나는 JSX 리액트 엘리먼트를 하나 넘기고 두번쨰는 mountNode를 넘기고 있다. 실제로 위 코드는 바벨을 만나면 아래 코드로 변환이 된다.

```
ReactDOM.render(React.createElement('div', {id: 'msg'}, 'Hello World'), mountNode);
```

React.createElement를 호출하면서 3개의 값을 넘겨주는데 div와 객체로 변한 id 그리고 문자열 값을 넘겨준다. 첫번째 인자로 넘겨진 것들이 이와 같이 바뀌어버렸다.

즉 `<div id="msg">Hello World</div>` 이 코드가 `React.createElement('div', {id: 'msg'}, 'Hello World')` 이렇게 바뀐 것이다.

두번째 케이스를 보자.

```
<div id={if (condition) { 'msg' }}> Hello World </div>

React.createElement('div', {id: if (condition) { 'msg' }}, 'Hello World')

React.render(<div id={condition ? 'msg' : null}>Hello World</div>);
```

첫번째 코드를 사용해본 적 있을까 ? id에 if문이 들어가 있다. 실제로 저런식으로 작성한 적이 있을까 ? 아마 없을 것이다. 문법오류를 일으키기 때문이다.  
왜 문법오류일까 ? if문이기 때문이다. if문은 값이 될 수 있을까 ? 없다. 그냥 if문은 값이 아니고 실행하는 '문' 이다. 값이 아니라 '문'이기 때문에 문법오류이다.
하지만 세번째 코드는 어떨까 ? `<div id={condition ? 'msg' : null}` 삼항연사자를 사용하고 있다. 이 삼항연산자는 무엇일까? 바로 '식' 이다. 식은 결국 계산이 끝나고 나면 '값'으로 귀결된다. 그럼 결국 식은 값이 될 수 있다. 그래서 문제가 없이 실행이 되는 것이다.

그치만 그런데도 불구하고 if문을 값처럼 쓰고 싶어하는 사람이 있을 수 있다. 아래와 같이는 사용할 수 있다.

```
{(()=> {
    switch (this.state.color){
        case 'red':
            return '#ff0000';
        case 'green':
            return '#00ff00';
        case 'blue':
            return '#0000ff';
        default:
            return '#ffffff';
    }
})()}
```

이런 함수는 즉시실행함수로 IIFE라고 하는데 중괄호 내부에 값과 식만 넣어야 하는데 바로 값을 리턴하기 때문에 switch문을 사용할 수 있는 것이다.  
하나의 예시를 더 알아보자

```
{(()=>{
    const rows = [];

    for( let i = 0; i < objectRows.length; i++ ){
        rows.push(<ObjectRow key={i} data={objectRows[i]} />);
    }
    return rows;
})()}
```

위 즉시실행함수를 보자. React를 사용하면서 위와같은 코드를 이제 작성하지 않게 되었지만 실제로 내가 예전에 많이 짯던 코드방식이다.  
위 코드의 로직이 좋지않다. 임시변수 rows를 선언하여 반복문을 돌면서 ObjectRow컴포넌트를 계속 push로 누적시키고 있다. 그리고 마지막으로 rows를 return하고 있다.
ObjectRow를 여러번 그리고 싶을 때 꼼수를 부린 것이다. 그럼 어떻게 해야하나. 바로 고차함수를 사용하는 것이다. 우리가 자주 사용하는 map, filter, reduce 등이 바로 고차함수중 하나이다.

```
{objectRows.map((obj, i) => (
    <ObjectRow ket={i} data={obj} />
))}
```

이렇게 간단하게 위 코드를 수정할 수 있다.
