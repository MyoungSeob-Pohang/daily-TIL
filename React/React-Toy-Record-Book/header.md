# MyHeader 컴포넌트

상단 부분 공통 Header를 작성하기 위한 MyHeader컴포넌트 작성. 중앙 부분은 텍스트로 고정하고 왼쪽 오른쪽은 버튼으로 사용한다.
객체로 값 받아오는 것을 잊지말도록 하자. 호출은 Main 에서 작성하였다.

```
// 부모 컴포넌트에서 왼쪽, 중앙텍스트, 오른쪽 값을 받아와서 객체로 받음
const MyHeader = ({ leftChild, headText, rightChild }) => {
    return (
        <header>
            {/* 받아온 각각의 값을 지정 */}
            <div className="head_left_btn">{leftChild}</div>
            <div className="head_text">{headText}</div>
            <div className="head_right_btn">{rightChild}</div>
        </header>
    );
};
export default MyHeader;
```

호출

```
<MyHeader
    // MyButton컴포넌트를 호출하여 < 텍스를 가진 버튼 구현 및 달 감소 함수를 그대로 내려주어 사용
    leftChild={<MyButton text={'<'} onClick={decreaseMonth} />}

    // MyButton컴포넌트를 호출하여 중앙 텍스트 구현
    headText={`${curDate.getFullYear()}년 ${curDate.getMonth() + 1}월`}

    // MyButton컴포넌트를 호출하여 > 텍스를 가진 버튼 구현 및 달 증가 함수를 그대로 내려주어 사용
    rightChild={<MyButton text={'>'} onClick={increaseMonth} />}
/>
```
