# RecordList-2

작성하다 보니 더미데이터가 필요한거 같아서 App에서 더미데이터 5개를 생성해 주었다.

```
const dummyData = [
    { id: 1, comment: '힘들다1', date: 1644241008374, subject: 'HTML', studyTime: 3 },
    { id: 2, comment: '힘들다2', date: 1644241008376, subject: 'CSS', studyTime: 2 },
    { id: 3, comment: '힘들다3', date: 1644241008379, subject: 'JavaScript', studyTime: 1 },
    { id: 4, comment: '힘들다4', date: 1644241008380, subject: 'React', studyTime: 2 },
    { id: 5, comment: '힘들다5', date: 1644241008382, subject: 'JavaScript', studyTime: 8 },
];
```

더미데이터가 들어오고 나니 문제점들이 보여서 하나하나 고쳐보았다, 일단 과목별 필터링이 제대로 되지않고 있엇다, 코드도 난독화 되있어서 보기 힘들길래 if-elseif를 switch로 묶어주고 잘못된 코드를 수정했다, 기존에는 item.subject = 'HTML' 이런식으로 코드가 작성되어 있어서 과목별 필터를 선택하면 모든 과목이 선택한 필터로 바뀌어버렸다.

```
const getProcessedRecordList = () => {
        const filterCallBack = (item) => {
            switch (filter) {
                case 'HTML':
                    return item.subject === 'HTML';
                case 'CSS':
                    return item.subject === 'CSS';
                case 'JavaScript':
                    return item.subject === 'JavaScript';
                case 'React':
                    return item.subject === 'JavaScript';
                default:
                    return filter;
            }
        };

        const compare = (a, b) => {
            if (sortType === 'latest') {
                return parseInt(b.date) - parseInt(a.date);
            } else {
                return parseInt(a.date) - parseInt(b.date);
            }
        };

        const copyList = JSON.parse(JSON.stringify(recordList));
        const filteredList = filter === 'ALL' ? copyList : copyList.filter((it) => filterCallBack(it));

        const sortedList = filteredList.sort(compare);
        return sortedList;
    };
```

filterCallBack 함수를 이렇게 수정하고 보니 조금더 읽기 쉬운 코드가 된거같아 뿌듯하였다. 그리고 총 공부시간과 이번달 공부시간을 표시하는 부분을 컴포넌트안에 만들었다.

```
{/* 공부시간 표시를 위한 영역 */}
<div className="total_wrap">
    <div>이번 달 공부시간 : {study_month_time_all} 시간</div>
    <div>전체 공부시간 : {study_time_all} 시간</div>
</div>
```

연산코드는 아래와 같다.

```
// 이번달 전체 공부시간을 받아오기 위한 연산
const studyTime = recordList.map((it) => it.studyTime);
const study_month_time_all = studyTime.reduce((acc, cur) => acc + cur, 0);

// 전체 공부시간을 받아오기 위한 연산
const allStudyTimeList = useContext(RecordStateContext);
const allStudyTime = allStudyTimeList.map((it) => it.studyTime);
const study_time_all = allStudyTime.reduce((acc, cur) => acc + cur, 0);
```

일단 동작은 한다. 정확하게 리액트 스럽게 코드를 작성했을까 생각해보면 아닌 것같은 느낌이 마구 든다, 솔직히 아직은 어떻게 바꿔야 할지 모르겠다.
이번달 공부시간은 RecordList가 받아오는 props에서 map을 통해 시간만 뽑아낸 뒤 그걸 reduce로 연산하였고 ,  
전체 공부시간은 app.js가 data를 관리하고 있으니 useContext로 가져와서 역시 map, reduce 순서로 계산하였다.  
실제 작동은 달이 바뀌면 전체 공부 시간은 계속유지 되었고 해당 월 공부시간은 초기화 되는 것을 볼 수 있었다.
