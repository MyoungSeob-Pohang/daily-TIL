# Main - 1

메인에서 Header은 좌우 달 변경 버튼과 가운데 현재 날짜를 보여주는 역할이다. MyHeader컴포넌트를 호출하여 좌측은 달 감소 버튼으로 우측은 달 증가 버튼으로 가운데는 현제 달을 보여줄 수 있도록 구현하였다.

1. 날짜데이터를 관리할 상태를 선언한다. 초기값은 현재시간
2. MyHeader컴포넌트에 왼,우측 자식으로 MyButton컴포넌트를 전달하여 각각 감소, 증가 함수를 호출하도록 하여 prop으로 내려준다.

getFullYear은 년도, getMonth은 달이지만 0부터 시작하여 + 1을 하여야 현재 달이 출력된다. getDate는 현재 몇일인지 알려준다.

```
import MyHeader from '../components/MyHeader';
import MyButton from '../components/MyButton';
import { useState } from 'react';

const Main = () => {
    // 날짜데이터를 관리할 상태 , 초기값은 현재시간
    const [curDate, setCurDate] = useState(new Date());

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
        </div>
    );
};
export default Main;
```
