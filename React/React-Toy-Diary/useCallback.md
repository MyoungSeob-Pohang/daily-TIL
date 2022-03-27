# 최적화 3 - useCallback

## 코드

[React-Diary](https://github.com/MyoungSeob-Pohang/React-Diary)

컴포넌트는 본인이 가진 state가 변경되거나, 부모컴포넌트가 리렌더링이 될 때, 그리고 자신이 받은 prop이 변화될 때 리렌더링이 일어난다.  
지금 DiaryEditor컴포넌트에선 onCreate함수를 props로 받고 있다. 이 함수는 일기 저장하기를 눌렀을 때 data의 값을 추가할 때 사용한 함수이다.  
그래서 부모 컴포넌트 App에서 시작 하자마자 2번의 렌더링이 일어난다.

```
const [data, setData] = useState([]);
```

일단 data state가 빈 배열로 초기화 되면서 렌더링이 일어나고, 부모가 렌더링이 일어났으니 DiaryEditor컴포넌트도 렌더링이 일어난다.  
그리고 App컴포넌트 에서 Mount시점에 호출한 getData()함수 호출로 setData를 호출해서 새로운 값을 전달하면서 data state가 한번 더 바뀌게 된다.
그래서 다시 렌더링이 일어나고 DiaryEditor컴포넌트도 역시 렌더링이 일어난다. 2번 일어나는 것이다. 역시 이 부분도 초기화 할 수 있을 듯 하다.  
일기 저장하기 버튼을 누르지 않았는데 부모 컴포넌트인 App이 onCreate함수를 가지고 있고 그것을 자식인 DiaryEditor컴포넌트에 내려주니 자동으로 DiaryEditor컴포넌트가 렌더링 되는 것이다. 비효율적이니 onCreate함수를 다시 생성되지 않게 해야할 거 같은데 아마 useMemo를 사용하면 되지 않을까 ? 하지만 useMemo는 사용하면 안된다.  
[최적화 / 연산 결과 재사용](https://github.com/MyoungSeob-Pohang/daily-TIL/blob/main/React-Toy-Diary/useMemo.md)
문서에서 작성해 두었듯이 useMemo를 사용하게 되면 함수 자체가 값으로 변경되어 버린다. 지금 onCreate함수는 원본 그대로 DiaryEditor에게 보내주는데 onCreate함수가 값을 반환하는 것을 원하지 않고 그래서도 안된다.  
그렇기 때문에 새로운 React기능을 활용하는데 그것이 바로 useCallback이다.  
useCallback의 사용방법은 아래와 같다.

```
const memoizedCallback = useCallback(() => {
        doSomething(a, b)
    }, [a, b]
);
```

역시 다른 최적화인 useMemo나 useEffect 와 사용방법이 동일하다. 첫번 째인자로 콜백함수 두번째 인자로 뎁스를 전달하고 있다.  
useCallback의 기능은 메모이제이션된 콜백을 반환한다. 즉 값을 반환하는게 아니라 콜백함수를 다시 반환하는 역할을 하는 것이다.
그럼 이것을 onCreate함수에 적용시켜보자.

```
const onCreate = useCallback((author, content, emotion) => {
        const create_date = new Date().getTime();

        const newItem = {
            author,
            content,
            emotion,
            create_date,
            id: dataId.current,
        };
        dataId.current += 1;
        setData([newItem, ...data]);
    }, []);
```

첫번째 인자로 전달한 콜백함수는 DiaryEditor이 일기 저장하기를 눌렀을 때 데이터를 추가하는 함수가 되고, 두번째 인자로 뎁스는 빈배열을 전달하여 Mount되는 시점에 한번만 만들고, 그 다음부터는 첫번째 만든 함수를 재사용할 수 있도록 하였다.  
그리고 결과를 확인하니 다행이 한번만 렌더되고 최적화가 잘 된 모습을 볼 수 있었다. 하지만 일기를 추가하니 이상한일이 발생하였다. 바로 다른 모든 일기가 삭제되어 버리고 내가 방금 추가한 일기만 멀쩡히 살아있엇다. 이 이유는 useCallback를 활용하면서 뎁스에 아무값도 넣어주지 않아서 그렇다.  
이게 무슨 뜻이냐면, onCreate함수는 Mount되는 시점에 한번만 생성되기 때문에 그 당시에 data state값이 [] 빈 배열이다. 빈 배열이기 때문에 onCreate함수가 가장 마지막으로 생성되었을 때 빈 배열이었으니 나머지 값은 삭제가 되고 내가 추가한 하나만 생성이 되는 것이다. 그래서 아무리 추가하여도 1개 이상 추가 될 수 없다.

**함수는 컴포넌트가 재생성 될때 다시 생성되는 이유가 있다. 현재의 state값을 참조할 수 있어야 하기 때문이다. 그런데 onCreate함수는 콜백안에서 뎁스를 빈배열로 전달했기 때문에 이 onCreate함수가 알고 있는 data의 값은 [] 빈배열 밖에없다.**

그래서 결론적으로 뎁스에 data를 넣어주어야 한다. 그런데 이 뎁스는 아까도 말했듯이 data에 있는 값이 변경되면 onCreate함수를 재생성 하게 되는데 이렇게되면 또 문제가 useCallback함수를 쓰기전과 동일하게 되어버린다. 한번만 호출이되어야 하는데 계속 호출이 되는 것이다.

이럴때는 함수형 업데이트라는 것을 사용한다. 함수형 업데이트란 setData에 값이 아니라 함수를 전달하는 것이다. 이 setData에 화살표 함수를 추가하여 data를 전달하고 받아서 아이템을 추가한 데이터를 리턴하는 콜백함수를 setData에 전달하는 것이다.
이렇게 setState함수에 함수를 전달하는 것을 함수형 업데이트라 표현하는데, 이렇게 하면 뎁스를 비워도 항상 최신의 state를 data 인자를 통해 참고할 수 있게 되면서 뎁스를 비울수 있게 된다.

```
const onCreate = useCallback((author, content, emotion) => {
        const create_date = new Date().getTime();

        const newItem = {
            author,
            content,
            emotion,
            create_date,
            id: dataId.current,
        };
        dataId.current += 1;
        setData((data) => [newItem, ...data]);
    }, []);
```

```
setData((data) => [newItem, ...data]);
```

이렇게 되면 이제 처음에 한번만 렌더가 되고 일기 리스트에서 삭제/수정을 하여도 DiaryEditor은 리렌더링 되지 않는다.
