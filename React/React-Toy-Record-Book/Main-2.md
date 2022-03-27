# Main - 2

메인을 마저 구현하자.

일단 전체 데이터관리할 상태를 선언하고 App컴포넌트에서 내려오는 data를 받기위해 context도 선언하였다.  
useEffect를 사용하여 전체리스트와 날짜가 바뀔 때 해당 월의 첫날과 마지막날을 정해서 그 달에 해당하는 아이템만 뽑아올 수 있게 하였다.  
그리고 전체 데이터를 RecordList를 만들어서 props로 넘겨주었다.

```
import MyHeader from '../components/MyHeader';
import MyButton from '../components/MyButton';
import { useContext, useEffect, useState } from 'react';
import { RecordStateContext } from '../App';
import RecordList from '../components/RecordList';

const Main = () => {
    // 날짜데이터를 관리할 상태 , 초기값은 현재시간
    const [curDate, setCurDate] = useState(new Date());

    // 기록장 데이터 관리할 상태 초기값은 빈배열
    const [data, setData] = useState([]);

    // App컴포넌트에서 내려오는 data를 받기 위한 Context
    const recordList = useContext(RecordStateContext);

    useEffect(() => {
        // 기록의 길이가 1개보다 많아야 조건 진행
        if (recordList.length >= 1) {
            const firstDay = new Date(curDate.getFullYear(), curDate.getMonth(), 1).getTime();
            const lastDay = new Date(curDate.getFullYear(), curDate.getMonth() + 1, 0, 23, 59, 59).getTime();

            // 그걸 setData에 전달하여 새로운 데이터 생성
            setData(recordList.filter((it) => firstDay <= it.date && it.date <= lastDay));
        }
        // curDate는 달이 변경되면 당연히 상태변화해야 되니 넘겨주고 , recordList도 달이 바뀌면서 각 달에 맞는 기록을 뽑아야 하기에 같이 전달
    }, [recordList, curDate]);

    // 달 감소 함수
    const decreaseMonth = () => {
        setCurDate(new Date(curDate.getFullYear(), curDate.getMonth() - 1, curDate.getDate()));
    };

    // 달 증가 함수
    const increaseMonth = () => {
        setCurDate(new Date(curDate.getFullYear(), curDate.getMonth() + 1, curDate.getDate()));
    };
    return (
        <div className="Main">
            {/* My버튼 컴포넌트 호출 하여 버튼 및 텍스트 구현 */}
            <MyHeader
                // MyButton컴포넌트를 호출하여 < 텍스를 가진 버튼 구현 및 달 감소 함수를 그대로 내려주어 사용
                leftChild={<MyButton text={'<'} onClick={decreaseMonth} />}
                // MyButton컴포넌트를 호출하여 중앙 텍스트 구현
                headText={`${curDate.getFullYear()}년 ${curDate.getMonth() + 1}월`}
                // MyButton컴포넌트를 호출하여 > 텍스를 가진 버튼 구현 및 달 증가 함수를 그대로 내려주어 사용
                rightChild={<MyButton text={'>'} onClick={increaseMonth} />}
            />
            {/* 전체 리스트 컴포넌트 호출하면서 props로 전체 data를 내려줌 */}
            <RecordList recordList={data} />
        </div>
    );
};
export default Main;
```
