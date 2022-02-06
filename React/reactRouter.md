# React Router

React Router는 CSR을 도와주는 라이브러리로 여러가지 기능을 가지고 있다.

1. <BrowserRouter></BrowserRouter>
   BrowserRouter는 그 안에 존재하는 컴포넌트들과 url매핑이 이루어져 있다라고 생각하면 된다.

2. <Routes></Routes>
   어떤 컴포넌트들을 url과 묶을 것인지에 대한 부모요소 이다.

3. <Route></Route>
   각 url과 묶여서 어떤 컴포넌트들을 가져 올지에 대한 경로 및 컴포넌트 설정. Routes의 자식요소 path와 element를 가진다.

위 기능들을 사용하여 url 매핑을 하게 되면 아래코드와 같다.

```
return (
    <BrowserRouter>
        <div className="App">
            <h2>App.js</h2>
            <Routes>
                <Route path="/" element={<Home />} />
                <Route path="/new" element={<New />} />
                <Route path="/edit" element={<Edit />} />
                <Route path="/diary/" element={<Diary />} />
            </Routes>
        </div>
    </BrowserRouter>
);
```

BrowserRouter로 전체를 감싸주고 Routes안에 Route를 배치하여 각 path에 맞는 element를 설정해주면 된다. path는 경로 element는 컴포넌트이다.  
Routes에 감싸지지 않은 <h2>App.js</h2> 요소는 모든 페이지에서 나타난다.

4. a링크로 페이지 이동하지 않음
   보통 html에서 a태그로 페이지 이동을 많이 하는데 리액트도 사용이 가능하지만, 만약 a링크로 모든페이지를 이동한다면 MPA와 동일하게 새로고침이 일어난다.  
   그래서 리액트는 보통 Link컴포넌트를 사용하는데 방법은 아래와 같다.

```
<Link to ={'/'}>HOME</Link>
```

이렇게 사용하며, 이 Link를 사용하여야지 페이지 새로고침 없이 빠르게 이동할 수 있다.  
**하지만 SPA방식 이기때문에 실제 페이지 이동이라기 보다는 페이지를 업데이트 했다 라고 볼 수있다. 사용자가 느끼는 것은 페이지가 이동된 것과 동일하다.**
