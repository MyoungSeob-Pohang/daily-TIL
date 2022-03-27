# Record

이번엔 기록장 상세보기 페이지를 만들어보자. 일단 이전 코드들을 보면 RecordItem컴포넌트에서 글 목록 클릭 상세보기로 이동하는데 그 부분의 코드가 아래와 같다.

```
const goDetail = () => {
    navigate(`/record/${id}`);
};

{/* 클릭 시 상세보기로 이동하며 받아온 것들을 보여줌 */}
<div className="info_wrap" onClick={goDetail}>

    <div className="record_date">{strDate}</div>
    <div className="study_time">{studyTime}시간</div>
    <div className="subject">{subject}</div>
    <div className="comment_preview">{comment.slice(0, 25)}</div>

</div>
```

코드를 보면 goDetail함수를 호출하여 record로 페이지 이동 시 /${id}로 url에 아이디 값을 담아서 넘기고 있기 때문에 일단 id값 부터 받아주어야 한다.  
useParam를 사용하여 url에 있는 id값을 들고와서 담아주었다.

```
const { id } = useParams();
```

그리고 전체 Record리스트를 가지고 있는 App에서 전체 데이털를 가져오기 위해 useContext를 사용해 데이터를 가지고 왔다.

```
const recordList = useContext(RecordStateContext);
```

그 후, recordList의 상태 관리를 위한 useState도 하나 선언해 주었다.

```
const [data, setData] = useState();
```

이렇게 되면 준비 끝이다. recordList의 기록이 있는지 알아보고 기록이 1개 이상있다면 그 기록의 아이디 값과 useParams로 가져온 id와 비교를 하여 같은 기록을 찾으면 된다. 그 기록을 찾아 setData에 넣어주고 가져다 쓰면 그만인 페이지 이다. 한번 알아보자.

```
useEffect(() => {
    if (recordList.length >= 1) {
        const targetRecord = recordList.find((it) => parseInt(it.id, 10) === parseInt(id, 10));

        if (targetRecord) {
            setData(targetRecord);
            return;
        }

        alert('없는 기록 입니다.');
        navigate('/', { replace: true });
    }
}, [id, recordList, navigate]);
```

일단 useEffect 를 사용하여 recordList가 비어있지 않은지 확인하고 find를 사용하여 아이디가 같은 개체를 찾아서 targetRecord에 담아 두었다. 그 후 targetRecord에 데이터가 존재한다면 setData를 해서 값을 지정해 주었고 그렇지 않다면 없는 기록입니다 를 출력 후 메인페이지로 돌아가게 작업해 두었다. 이때 useEffect의 변하는 값은 id값, 전체 기록의 데이터인 recordList, 그리고 페이지전환 할때 변경되는 navigate이다.

그럼 recordList에 기록이 있고 id가 일치하는게 있으면 data에 저장이되었을 것이니 호출해서 값을 뽑아보면 된다.

```
if (data) {
        return (
            <div className="Record">
                <MyHeader
                    leftChild={<MyButton text={'뒤로가기'} onClick={() => navigate(-1)} />}
                    headText={`${new Date(data.date).toLocaleDateString()}의 기록`}
                    rightChild={<MyButton text={'수정하기'} onClick={() => navigate(`/edit/${data.id}`)} />}
                />

                <section>
                    <h4>오늘의 기록</h4>
                    <p>공부한 날짜 : {new Date(data.date).toLocaleDateString()}</p>
                    <p>공부하신 시간 : {data.studyTime} 시간</p>
                    <p>공부한 과목 : {data.subject}</p>
                </section>

                <section>
                    <h4>한줄평</h4>
                    <p>{data.comment}</p>
                </section>
            </div>
        );
    }
```

data가 존재한다면 header를 만들고 각 기록을 section으로 나누어서 출력해 주었다. 그리고 데이터가 없을 때는

```
return <div className="NF404">Not Found 404</div>;
```

이런식으로 리턴해 주었다. 어차피 데이터가 없을때는 useEffect에 걸려서 메인으로 돌아갈 것이라서 저 글은 보이지 않을 것이다.
