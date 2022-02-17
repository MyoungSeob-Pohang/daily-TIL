# RecordEditor

New컴포넌트를 작성하기 위해서 제작하려고 하였으나, New컴포넌트와 Edit컴포넌트는 거의 형식이 비슷해서 하나로 분리하기로 했다.  
그게 RecordEditor 컴포넌트이다. 그래서 New에서는 바로 RecordEditor컴포넌트를 호출한다.

```
import RecordEditor from '../components/RecordEditor';

const New = () => {
    return (
        <div className="New">
            <RecordEditor />
        </div>
    );
};
export default New;
```

RecordEditor컴포넌트는 주석으로 한줄씩 설명하였다. input type 중 time이라는 것을 활용하여 공부 시작시간과 종료시간을 받았으며 이 time을 사용할때 초기값을 어떤 것을 줘야하는지 몰라 에러가 났엇는데 '00:00:00' 식으로 포맷을 정해주니 문제가 없어졌다, 그리고 min max값도 정해줘야 한다는 것을 검색으로 알아내어 그 부분도 작성해주었다. subject과목 부분은 이전에 RecordList컴포넌트에서 만들어둔 ControlBox컴포넌트를 export 하여 select/option값을 정해주었다. 아침에 시간이 많이 없어서 handleSubmit 함수를 구현 못하였는데 내일 마무리하도록 하자.

```
import { useContext, useEffect, useRef, useState } from 'react';
import { useNavigate } from 'react-router-dom';
import { RecordDispatchContext } from '../App';
import MyButton from './MyButton';
import MyHeader from './MyHeader';
import { ControlMenu } from './RecordList';

const RecordEditor = () => {
    // 이동 네비게이트
    const navigate = useNavigate();
    // 시간 상태를 담기위한 상태선언 , 객체로 선언하여서 2개값을 받을 수 있게함
    const [date, setDate] = useState({
        startTime: '00:00:00',
        endTime: '00:00:00',
    });
    // 과목 상태값
    const [subject, setSubject] = useState('');
    // onCreate함수를 사용하기 위한 Context
    const { onCreate } = useContext(RecordDispatchContext);

    // textarea값을 감을 상태값
    const [comment, setComment] = useState('');
    // textarea Ref
    const commentRef = useRef();

    // 과목선택 option 값 리스트
    const subjectOption = [
        { value: 'HTML', name: 'HTML' },
        { value: 'CSS', name: 'CSS' },
        { value: 'JavaScript', name: 'JavaScript' },
        { value: 'React', name: 'React' },
    ];

    // 작성완료 함수
    const handleSubmit = () => {
        console.log(comment.length);
        if (comment.length < 5) {
            commentRef.current.focus();
            return;
        }

        if (window.confirm('기록을 작성 하시겠습니까?')) {
            // date, comment, studyTime, subject
            onCreate(comment, subject);
        }
    };

    return (
        <div className="RecordEditor">
            {/* 상단 헤더 작성 */}
            <MyHeader leftChild={<MyButton text={'< 뒤로가기'} onClick={() => navigate(-1)} />} headText={'새 기록작성'} />

            <div>
                {/* 과목선택 부분 ControlMenu를 List컴포넌트에서 가지고옴 */}
                <section>
                    <h4>과목 선택</h4>
                    <ControlMenu value={subject} onChange={setSubject} optionList={subjectOption} />
                </section>

                {/* 시작시간, 종료시간. min-max값 설정해 둬야하고 spread를 사용하여 상태값을 가지고오고 name값으로 상태값을 업데이트 시켰다 */}
                <section>
                    <div className="start_time">
                        <h4>시작 시간</h4>
                        <div className="input_box">
                            <input
                                className="input_date"
                                type="time"
                                value={date.startTime}
                                min="00:00:00"
                                max="23:59:59"
                                name="startTime"
                                onChange={(e) => setDate({ ...date, [e.target.name]: e.target.value })}
                            />
                        </div>
                    </div>
                    <div className="end_time">
                        <h4>종료 시간</h4>
                        <div className="input_box">
                            <input
                                className="input_date"
                                type="time"
                                value={date.endTime}
                                min="00:00:00"
                                max="23:59:59"
                                name="endTime"
                                onChange={(e) => setDate({ ...date, [e.target.name]: e.target.value })}
                            />
                        </div>
                    </div>
                </section>

                {/* 한줄평 작성 */}
                <section>
                    <div className="today_comments">
                        <textarea
                            ref={commentRef}
                            placeholder="오늘의 한줄평"
                            value={comment}
                            onChange={(e) => setComment(e.target.value)}
                        />
                    </div>
                </section>

                {/* 버튼 영역 */}
                <section>
                    <div className="control_box">
                        <MyButton text={'취소하기'} onClick={() => navigate(-1)} />
                        <MyButton text={'작성완료'} type={'positive'} onClick={handleSubmit} />
                    </div>
                </section>
            </div>
        </div>
    );
};

export default RecordEditor;
```
