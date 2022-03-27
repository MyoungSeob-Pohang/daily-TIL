# 상세 Diary 코드 상세 리뷰 < 개요 및 라우트 >

## 페이지 구성

1. Home : 해당 하는 년/월에 맞는 전체 일기목록을 보여주는 페이지
2. New : 새로운 일기를 작성하는 페이지
3. Edit : 작성 한 일기를 수정하는 페이지
4. Diary : 작성한 일기를 상세히 보는 페이지

일기 뿐만 아니라 거의 모든 시스템이 위와같은 페이지들을 가진다. 특히 게시판의 경우도 동일하다. CRUD 시스템은 위와 같은 구성을 가진다.  
일단 화면을 전체적으로 만들기 전 어떤식으로 구성해 나갈 것인지 적어보는게 중요한데, 최상위 부모인 App컴포넌트에서 모든 하위 컴포넌트를 가지고 있고, 필요한 데이터와 함수를 자식 컴포넌트에게 내려줄 것이다.  
그럼 App은 부모가 되고 자식은 New, Edit, Diary가 된다. 일단 라우터로 연결을 해준다.

1. 라우터 연결

```
<BrowserRouter>
    <div className="App">

    </div>
</BrowserRouter>
```

BrowserRouter는 리액트 SPA환경에서 가장 많이 쓰이는 라우터중 하나인 react-router-dom패키지의 구성요소 중 하나로 history API를 사용해 URL과 UI를 동기화하는 라우터입니다.

history API : 브라우저에서 페이지를 로딩하면 세션 히스토리 라는 것을 가지게 되는데 세션 히스토리는 페이지를 이동할 때 마다 쌓이게 되며, 이를 통해 뒤로가기 나 앞으로 가기 등의 기능을 가능하게 한다.

세션 : 여러 페이지에 걸쳐 사용되는 사용자의 정보를 저장하는 방법을 의미, 서버 측에 데이터를 저장하고, 세션의 키값만을 클라이언트 측에 남겨둡니다.  
브라우저는 필요할 때마다 이 키값을 이용하여 서버에 저장된 데이터를 사용하게 됩니다.
이러한 세션은 보안에 취약한 쿠키를 보완해주는 역할을 하고 있습니다.

2. Routes와 Route를 사용하여 각 path(경로)에 맞는 컴포넌트를 매칭시킨다.

```
<Routes>
    <Route path="/" element={<Home />} />
    <Route path="/new" element={<New />} />
    <Route path="/edit" element={<Edit />} />
    <Route path="/diary/:id" element={<Diary />} />
</Routes>
```

Routes : 여러개의 Route를 묶기 위해 사용  
Route : 현재 주소창의 경로와 매치될 경우 보여줄 컴포넌트 지정

즉 url에 / 아무것도 없으면 Home컴포넌트를, /edit 이면 Edit컴포넌트를 보여달라는 말이 된다.

BrowserRouter, Route, Routes 모두 react-router-dom패키지를 사용한다.

전체코드

```
return (
    <BrowserRouter>
        <div className="App">
            <Routes>
                <Route path="/" element={<Home />} />
                <Route path="/new" element={<New />} />
                <Route path="/edit" element={<Edit />} />
                <Route path="/diary/:id" element={<Diary />} />
            </Routes>
        </div>
    </BrowserRouter>
)
```
