# RecordList

리스트는 메인페이지에서 모든 아이템목록을 뽑아서 볼 수 있는 리스트이며 눌렀을 때 상세보기 페이지로 이동하는 인터렉션이 있다.
일단 정렬과 필터링을 위한 작업을 해주고, 전체 아이템은 정렬된 함수의 map함수를 통해서 RecordItem컴포넌트로 전달해 주었다.
주석만 봐도 이해가 갈것이다.

```
import { useState } from 'react';
import { useNavigate } from 'react-router-dom';
import MyButton from './MyButton';
import RecordItem from './RecordItem';

// 내부 컴포넌트로 select안에 option 태그에 채워질 내용정의
const ControlMenu = ({ value, onChange, optionList }) => {
    return (
        <select className="ControlMenu" value={value} onChange={(e) => onChange(e.target.value)}>
            {optionList.map((it, idx) => (
                <option key={idx} value={it.value}>
                    {it.name}
                </option>
            ))}
        </select>
    );
};

// 필터링 옵션 객체 배열
const filterOption = [
    { value: 'ALL', name: 'ALL' },
    { value: 'HTML', name: 'HTML' },
    { value: 'CSS', name: 'CSS' },
    { value: 'JavaScript', name: 'JavaScript' },
    { value: 'React', name: 'React' },
];

// 정렬 옵션 객체 배열
const sortOption = [
    { value: 'latest', name: '최신순' },
    { value: 'oldest', name: '오래된 순' },
];

// 전체 data를 받을 recordList
const RecordList = ({ recordList }) => {
    // 페이지 이동을 위한 네비게이트
    const navigate = useNavigate();
    // 정렬 관리 상태
    const [sortType, setSortType] = useState('latest');
    // 필터링 상태
    const [filter, setFilter] = useState('ALL');

    // 정렬을 통해 아이템의 위치를 바꾸기 위한 함수
    // 전체 일기데이터를 copyList에 담고, filteredList에 필터된 값들을 담아서 그 값을 정렬하여 리턴한다.
    // 예를들어 최신순이고 HTML만 뽑았다면 filteredList에 HTML값을 가진 아이템만 담길 것이고 그것을 정렬하여 리턴하는 것
    const getProcessedRecordList = () => {
        const filterCallBack = (item) => {
            if (filter === 'HTML') {
                return (item.subject = 'HTML');
            } else if (filter === 'CSS') {
                return (item.subject = 'CSS');
            } else if (filter === 'JavaScript') {
                return (item.subject = 'JavaScript');
            } else if (filter === 'React') {
                return (item.subject = 'React');
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

    return (
        <div className="RecordList">
            <div className="menu_wrap">
                <div className="left_col">
                    {/* 초기선택값은 현재 정렬 값, 변경이벤트는 상태 변경 함수 옵션 리스트는 배열객체 */}
                    <ControlMenu value={sortType} onChange={setSortType} optionList={sortOption} />
                    <ControlMenu value={filter} onChange={setFilter} optionList={filterOption} />
                </div>
                <div className="right_col">
                    {/* 새 기록작성 페이지로 이동하는 버튼 */}
                    <MyButton type={'positive'} text={'새 기록작성'} onClick={() => navigate('/new')} />
                </div>
            </div>

            <div className="item_wrap">
                {/* 정렬된 아이템을  RecordItem컴포넌트에 전달*/}
                {getProcessedRecordList().map((it) => (
                    <RecordItem key={it.id} {...it} />
                ))}
            </div>
        </div>
    );
};

// 초기 recordList가 빈 배열일 수 있으니, defaultProps지정
RecordList.defaultProps = {
    recordList: [],
};

export default RecordList;
```
