# Home

Home컴포넌트는 메인이 되는 컴포넌트로 해당 월에 존재하는 모든 일기를 보여주고 월 변경이 가능하다.  
현재는 더미리스트를 받아와서 더미리스트로 전체 일기를 출력하고 있다.

```
import { useContext, useEffect, useState } from 'react';
import { DiaryStateContext } from '../App';
import MyButton from '../components/MyButton';
import MyHeader from './../components/MyHeader';

const Home = () => {
    const diaryDummyList = useContext(DiaryStateContext);
    const [data, setData] = useState([]);

    const [curDate, setCurDate] = useState(new Date());
    const headText = `${curDate.getFullYear()}년 ${curDate.getMonth() + 1}월 `;

    useEffect(() => {
        if (diaryDummyList.length >= 1) {
            const firstDay = new Date(curDate.getFullYear(), curDate.getMonth(), 1).getTime();
            const lastDay = new Date(curDate.getFullYear(), curDate.getMonth() + 1, 0).getTime();

            setData(diaryDummyList.filter((it) => firstDay <= it.date && it.date <= lastDay));
        }
    }, [diaryDummyList, curDate]);

    const increaseMonth = () => {
        setCurDate(new Date(curDate.getFullYear(), curDate.getMonth() + 1, curDate.getDate()));
    };

    const decreaseMonth = () => {
        setCurDate(new Date(curDate.getFullYear(), curDate.getMonth() - 1, curDate.getDate()));
    };

    return (
        <div className="Home">
            <MyHeader
                headText={headText}
                leftChild={<MyButton text={'<'} onClick={decreaseMonth} />}
                rightChild={<MyButton text={'>'} onClick={increaseMonth} />}
            />
        </div>
    );
};

export default Home;
```

1. const diaryDummyList = useContext(DiaryStateContext);
   App.js에서 보내는 Dummy를 받아오는 컨텍스트 선언이다. DiaryStateContext는 App에서 Provider로 data를 보내고 있고 그 data는 dummyData로 초기값이 설정되어있다.

2. const [data, setData] = useState([]);
   더미데이터 관리를 위한 상태이다.

3. const [curDate, setCurDate] = useState(new Date());
   월별 이동을 위한 상태관리이며 초기값으로 new Date()를 선언하여 현재 날짜와 시간을 초기값으로 curDate 전달한다.

4. const headText = `${curDate.getFullYear()}년 ${curDate.getMonth() + 1}월 `;
   MyHeader컴포넌트에 전달할 중앙 텍스트 값으로 현재 날짜와 시간을 가진 curDate에서 년도와 월을 뽑아서 headText에 저장해둔다. 자바스크립트에서 월은 -1 된 값이 나옴으로 + 1을 해주어 정확한 달을 가지고 온다.

5. useEffect로 diaryDummyList, curDate가 변경될 때마다 실행할 함수를 만든다. 그 함수의 작업내용은 더미리스트의 길이가 1이상인지 확인하여서 1 이상이라면,  
   curDate의 연도, 월을 전달하고 1을 전달하면 해당하는 월의 첫번째 날짜를 구해준다. 그 일자를 first에 담는다.
   그리고 마지막 날은 const lastDay = new Date(curDate.getFullYear(), curDate.getMonth() + 1, 0).getTime(); 이렇게 구할 수 있다.
   해당 월의 1일과 마지막날짜를 구했으니 더미데이터와 비교를 하면 된다. 즉 setData에는 더미데이터를 가지고와서 필터링을 하는데 첫번째 날과 마지막날 사이에 있는 일기만 가지고와서 Data값을 업데이트 해준다. 그러면 해당하는 달에 맞는 일기들만 뽑아서 출력할 수 있다. 그리고 일기의 데이터를 가진 diaryDummyList가 변경이 되면 기존에 출력되는 데이터도 달라지기에 같이 두번째 인자에 넣어준다.

```
useEffect(() => {
        if (diaryDummyList.length >= 1) {
            const firstDay = new Date(curDate.getFullYear(), curDate.getMonth(), 1).getTime();
            const lastDay = new Date(curDate.getFullYear(), curDate.getMonth() + 1, 0).getTime();

            setData(diaryDummyList.filter((it) => firstDay <= it.date && it.date <= lastDay));
        }
    }, [diaryDummyList, curDate]);
```

6. 월 증가 함수이다. 년 월 일을 가지고 와서 새로운 CurDate를 만든다.

```
const increaseMonth = () => {
    setCurDate(new Date(curDate.getFullYear(), curDate.getMonth() + 1, curDate.getDate()));
};
```

7. 월 감소 함수

```
const decreaseMonth = () => {
        setCurDate(new Date(curDate.getFullYear(), curDate.getMonth() - 1, curDate.getDate()));
    };
```

8. MyHeader컴포넌트에 text로 4번 headText을 넣어주고 좌 우에 각각 버튼을 정의 해준다.
