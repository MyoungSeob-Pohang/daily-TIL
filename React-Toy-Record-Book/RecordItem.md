# RecordItem

List에서 넘어온 값을 받아 각 아이템들을 정의하는 컴포넌트이다. 아이디 값과 받아온 내용 수정하기 버튼을 위치 시키고, 받아온 값을 view 시켜주는 컴포넌트이다.

```
import { useNavigate } from 'react-router-dom';
import MyButton from './MyButton';

// 전체아이템 객체로 받아옴
const RecordItem = ({ id, comment, date, studyTime, subject }) => {
    // 페이지 이동
    const navigate = useNavigate();
    // 데이트 포맷
    const strDate = new Date(parseInt(date)).toLocaleDateString();

    // 상세보기 이동
    const goDetail = () => {
        navigate(`/record/${id}`);
    };

    // 글 수정 이동
    const goEdit = () => {
        navigate(`/edit/${id}`);
    };

    return (
        <div className="RecordItem">
            {/* 순번 */}
            <div className="Number">{id}</div>

            {/* 클릭 시 상세보기로 이동하며 받아온 것들을 보여줌 */}
            <div className="info_wrap" onClick={goDetail}>
                <div className="record_date">{strDate}</div>
                <div className="study_time">{studyTime}</div>
                <div className="subject">{subject}</div>
                <div className="comment_preview">{comment.slice(0, 25)}</div>
            </div>

            {/* 수정페이지이동 */}
            <div className="btn_wrap">
                <MyButton text={'수정하기'} onClick={goEdit} />
            </div>
        </div>
    );
};
export default RecordItem;
```
