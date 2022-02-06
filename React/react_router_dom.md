# React Router Dom의 기능

REACT ROUTER V6 : CSR기반의 페이지 라우팅을 할 수 있게 해주는 라이브러리

1. Path Variable (useParams)
   경로의 변수를 사용하는 방법으로 예를들어 게시판의 글을 확인할때 /board/1 이런 경로를 많이 본적이 있을 것이다. 어떤 게시글을 읽을지에 대한 정보를 받아야하는데 1번 게시글인지 2번게시글인지 44번 게시글인지를 알아야 하는데, 이런 변수를 url에 같이 담아서 보내주는데 이것을 Path Variable이라고 한다.  
   useParams를 Hooks(사용자 정의 hooks)을 사용하는데 사용법은 아래와 같다.

```
<Route path="/diary/:id" element={<Diary />} />
```

path뒤에 보면 /:id가 보인다. id값을 넘겨준다는 것인데 저 넘겨준 id값은 다른 컴포넌트에서 const { id } = useParams(); 이런식으로 받아서 사용할 수 있다.
예를들어 www.naver.com/board/1 이라는 url이 전달되면 board컴포넌트에서 const { id } = useParams();을 사용하여 1값을 전달받아 사용할 수 있다.

2. Query String (useSearchParams)
   url과 함께 데이터를 전달하는 방법으로 보통 웹페이지에 데이터를 전달하는 가장 간단한 방법인데, url을 자세히보면 /edit?id=10&mode=dark 이런거 처럼 보이는 url들이 있다. 공개적으로 데이터를 전달하는 가장 간단한 방식으로 id값과 mode값을 각각 전달해주는 것이다. 이러한 형식을 Query String라고 한다.
   보통 리액트와 리액트 라우터를 사용하면 url에 Query String을 날려도 페이지가 잘 받아준다, 실제로 ?뒤에 있는 값들은 페이지이동에 영향을 주지 않기 때문이다.  
   이 값은 실제로 사용자가 받아서 사용을 해야하는데 사용법은 아래와 같다.

```
const [searchParams, setSearchParams] = useSearchParams();
const id = searchParams.get('id');
const mode = searchParams.get('mode');
```

이렇게 useSearchParams로 선언 후 get으로 받아온 값을 사용할 수 있다. 그런데 자세히보면 setSearchParams도 존재하는 걸 보니 값을 변경할 수도 있나본데 ? 정답이다.  
setSearchParams({id: 11, mode: white}) 이렇게 받아온 값을 그대로 바꾸거나 setSearchParams({who: moonsbi}) 받아온 값을 제외시켜 버리고 다른 것으로 대체할 수도 있다.

3. Page Moving(useNavigate)
   Link가 아닌 페이지 이동하는 방법으로,

```
<Link to ={'/'}>HOME</Link>
```

이런 방식이 아닌,

```
const navigate = useNavigate();

navigate('/');
```

이러한 방식이다. 훨씬 더 간편하고 뒤로가기 같은 기능도 -1 값을 넣어주면 쉽게 구현할 수 있다.

```
navigate(-1);
```
