# 공통 컴포넌트

Diary에서 공통적으로 사용되는 부분을 컴포넌트로 분리해보자 , 공통으로 사용하는 부분은 Header, 그리고 버튼 정의이다.

## Header

Header부분은 Diary 대부분의 최상단 위치에 존재하기 때문에 재사용을 위해 컴포넌트로 분리하였다.  
3부분으로 나누어 좌측에 버튼 중앙에 텍스트 우측에 버튼을 위치시킨다. Home에서 년월 변경시에 쓰인다.

```
const MyHeader = ({ headText, leftChild, rightChild }) => {
    return (
        <header>
            <div className="head_btn_left">{leftChild}</div>
            <div className="head_text">{headText}</div>
            <div className="head_btn_right">{rightChild}</div>
        </header>
    );
};

export default MyHeader;
```

headText라는 중앙에 삽입할 텍스트와 좌측,우측에 넣을 자식요소를 받아와서 header테그 안에 위치시켰다. 이렇게 하면

```
<div className="Home">
    <MyHeader
        headText={headText}
        leftChild={<MyButton text={'<'} onClick={decreaseMonth} />}
        rightChild={<MyButton text={'>'} onClick={increaseMonth} />}
    />
</div>
```

이런식으로 컴포넌트를 호출하면서 자식요소로 모든 것을 정해주어 쉽게 만들 수 있다. 지금 보면 MyButton이라는 컴포넌트도 호출하고 있는데 이 컴포넌트도 버튼 종류에 따라서 각각 다른 색상의 버튼이 나올 수 있도록 정의한 컴포넌트이다.

## Button

```
const MyButton = ({ text, type, onClick }) => {
    const btnType = ['positive', 'negative'].includes(type) ? type : 'default';

    return (
        <button className={['MyButton', `MyButton_${type}`].join(' ')} onClick={onClick}>
            {text}
        </button>
    );
};

MyButton.defaultProps = {
    type: 'default',
};
export default MyButton;
```

버튼의 텍스트와 Click이벤트 그리고 type를 받아온다. type는 버튼의 종류를 설정하는데 쓰이는데 ,

```
const btnType = ['positive', 'negative'].includes(type) ? type : 'default';
```

받아온 tpye이 positive이거나 negative인지 물어보고 맞으면 해당 tpye 아니면 default로 설정한다. 그리고, button className에 두번쨰 클래스로 정의해 주었는데 이렇게 하면 css에서 각 type마다 다른 css를 먹일 수 있다.

```
.MyButton {
    cursor: pointer;
    border: none;
    border-radius: 5px;
    padding: 10px 20px;
    font-size: 18px;
    white-space: nowrap;
    font-family: 'Nanum Pen Script';
}

.MyButton_default {
    background-color: #ececec;
    color: #000;
}

.MyButton_positive {
    background-color: #64c964;
    color: #fff;
}

.MyButton_negative {
    background-color: #fd565f;
    color: #fff;
}
```
