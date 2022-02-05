# 최적화 1 - 연산 결과 재사용 / useMemo

Memoization : 이미 계산 해 본 연산 결과를 기억 해 두었다가 동일한 게산을 시키면, 다시 연산하지 않고 기억 해 두었던 데이터를 반환 시키게 하는 방법.  
마치 시험을 볼 때 이미 풀어본 문제는 다시 풀어보지 않아도 답을 알고 있는 것과 유사함.

그럼 이 Memoization를 사용하기 위해 예시코드를 작성해보자.  
다이어리 프로젝트에 감정점수를 기록하는 emotion이 있는데, 이 emotion의 점수가 3점 이상이면 좋은일기로 분류하고, 3점보다 작으면 나쁜일기로 분류하자. 그리고 좋은 일기의 분류가 몇 %인지 계산하는 함수를 만들어보자.

```
const getDiaryAnalysis = () => {
        console.log('일기 분석 시작');

        const goodCount = data.filter((it) => it.emotion >= 3).length;
        const badCount = data.length - goodCount;
        const goodRatio = (goodCount / data.length) * 100;
        return { goodCount, badCount, goodRatio };
    };

    const { goodCount, badCount, goodRatio } = getDiaryAnalysis();
```

getDiaryAnalysis이라는 함수를 선언하여 시작할 때 분석시작 log를 찍은 후 filter를 이용하여 감정점수가 3점이상인 것의 갯수를 goodCount에 담았다.  
그리고 나쁜 일기는 전체 일기에서 좋은 일기의 갯수를 제외한 것이고, 좋은 일기의 비율은 전체갯수에서 좋은일기의 갯수를 나눈 후 \* 100을 한 값이다 . 그리고 그 값들을 리턴하였다. const { goodCount, badCount, goodRatio }에서 getDiaryAnalysis함수를 호출하여 그 값을 각각 담아 주었다.

그리고 App 컴포넌트에 작성을 해 주었는데 ,

```
<div className="App">
    {/* 조작 함수를 내려 줌 */}
    <DiaryEditor onCreate={onCreate} />

    <div>전체 일기 : {data.length} 개</div>
    <div>좋은 일기 : {goodCount} 개</div>
    <div>나쁜 일기 : {badCount} 개</div>
    <div>좋은 일기 비율 : {goodRatio}% </div>

    {/* 데이터만 내려 줌 */}
    <DiaryList onRemove={onRemove} diaryList={data} onEdit={onEdit} />
</div>
```

위와 같이 사용할 수 있게 되었다. 자 근데 이렇게 작성을 하고 나서 실제로 렌더링이 되는 것을 살펴보면, 우리가 일기의 본문을 수정할 때도 "일기 분석 시작"이라는 로그가 찍힌다. 왜냐하면 수정이 되면 컴포넌트 자체가 다시 렌더링이 되기 때문이고 그렇기 때문에 계속 "일기 분석 시작"가 값이 변할때 마다 찍히는 것이다. 하지만 우리의 일기 분석은 수정을 한다고 해서 emotion의 값이 바뀌는 것이 아니기 때문에 렌더링이 다시 되는 게 너무 비효율 적인거같다. 이럴때 사용할 수 있는 것이 바로 useMemo이다.

useMemo를 사용하게 된다면 아래와 같은 코드로 작성을 한다.

```
const getDiaryAnalysis = useMemo(() => {
        console.log('일기 분석 시작');

        const goodCount = data.filter((it) => it.emotion >= 3).length;
        const badCount = data.length - goodCount;
        const goodRatio = (goodCount / data.length) * 100;
        return { goodCount, badCount, goodRatio };
    }, [data.length]);

    const { goodCount, badCount, goodRatio } = getDiaryAnalysis;
```

useMemo의 사용법은 첫번째 인자로 콜백함수, 두번째 인자로 뎁스를 넘겨준다. 뎁스에 있는 값이 변할 때 콜백함수에 있는 명령들을 실행하는 것이다. useEffect와 사용방법이 동일하다.  
두번째 인자로 data.length를 넘겨주었는데 결국은 data.length가 변하지 않는 이상 getDiaryAnalysis 함수의 콜백함수는 실행되지 않는다, 즉 렌더링을 하지 않는다는 뜻이다. 여기서 또 주의할 점이 있는데, **useMemo를 사용하게 되면 더이상 함수가 아니게 된다.** 그냥 콜백함수가 리턴하는 값을 리턴하게 된다. 그래서 함수처럼 사용하는 것이 아니라 값처럼 사용해야 되기 때문에 const { goodCount, badCount, goodRatio } = getDiaryAnalysis; 이런식으로 그냥 값으로 사용해야 한다.  
실제로 확인해보면 길이가 변할 때, 즉 일기를 삭제할 때에 일기 분석 시작로그가 나오는 것을 알 수 있다.
