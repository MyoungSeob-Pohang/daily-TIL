# Button 컴포넌트

공통적으로 버튼 전체를 관리하기 위한 컴포넌트로, 긍정버튼 부정버튼, 디폴트 버튼으로 나뉜다.
글 작성, 수정 같은 버튼은 모두 positive, 삭제는 negative, 달 변경과 같은 애매한 버튼은 default로 클래스명을 지정해주었다.

```
// 부모 컴포넌트에서 텍스트와 타입, 이벤트를 받아옴
const MyButton = ({ text, type, onClick }) => {
    // 버튼 타입에 따른 클래스 변경을 위해 긍정적인 positive, 부정적인 negative과 아무것도 정해지지 않으면 default 지정될 수 있게 구현
    // positive는 초록색 , negative는 붉은색, default는 회색을 가지게 css 구현 예정
    const btnType = ['positive', 'negative'].includes(type) ? type : 'default';

    return (
        // 클래스 명 배열로 2개 지정 후 join으로 공백넣기 , 이벤트는 부모에게 내려온 이벤트 그대로 사용
        <button className={['MyButton', `MyButton_${btnType}`].join(' ')} onClick={onClick}>
            {text}
        </button>
    );
};
export default MyButton;
```
