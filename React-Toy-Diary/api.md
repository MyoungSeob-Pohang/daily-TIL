# API 호출하기

이번엔 useEffect를 사용하여 컴포넌트 Mount 시점에 API를 호출하여 API결과값을 일기 데이터의 초기값으로 사용해보자.
이번에도 json placeholder의 https://jsonplaceholder.typicode.com/comments 데이터를 활용하도록 하자.

일단 API 호출 함수를 만들어야 한다.

```
// API 호출 함수
    const getData = async () => {
        const res = await fetch('https://jsonplaceholder.typicode.com/comments').then((res) => res.json());

        // 20개 잘라내기
        const initData = res.slice(0, 20).map((it) => {
            return {
                author: it.email,
                content: it.body,
                emotion: Math.floor(Math.random() * 5) + 1,
                create_date: new Date().getTime(),
                id: dataId.current++,
            };
        });

        setData(initData);
    };
```

위 코드를 보면 비동기 함수로 만들어서 fetch는 동기적으로 사용하고 있다. 그럼 fetch가 끝나고 결과가 콜백함수로 들어가서 json형태로 읽어 온다.  
그리고 나서 initData에 res를 20개로 잘라 (500개가 있어 너무 많아서) map함수로 하나씩 순회하면서 email을 작성자로 body를 컨텐츠로 지정하고 emotion은 1~5 랜던값을 주었다. create_date는 현재시간 , id는 dataId 후위연산자를 통해 보낸 후 증가하게 끔 하였다. 그리고 그 결과를 setData에 넘겨주면 끝난다.

호출은 useEffect를 사용하여 Mount시점에 호출하도록 하자.

```
// Mount 시점에 API 호출
    useEffect(() => {
        getData();
    }, []);
```

이렇게 하면 가져온 API의 값으로 일기리스트 초기값을 설정할 수 있게된다.
