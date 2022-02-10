# Diary 상세보기

[New Edit](https://github.com/MyoungSeob-Pohang/daily-TIL/blob/main/Code-Review/Diary_Edit.md) 에서 이어지는 문서

Diary리스트에서 각 일기에 대한 상세보기 페이지를 구현하면 된다. 지금까지 했던 코드 구성과 별반 다를 것이 없다 하나하나 해석해보자.

```
import { useContext, useEffect, useState } from 'react';
import { useNavigate, useParams } from 'react-router-dom';
import { DiaryStateContext } from '../App';
import MyButton from '../components/MyButton';
import MyHeader from '../components/MyHeader';
import { getStringDate } from '../util/date';
import { emotionList } from '../util/emotion';

const Diary = () => {
    const { id } = useParams();
    const navigate = useNavigate();
    const diaryList = useContext(DiaryStateContext);
    const [data, setData] = useState();

    useEffect(() => {
        if (diaryList.length >= 1) {
            const targetDiary = diaryList.find((it) => parseInt(it.id) === parseInt(id));

            if (targetDiary) {
                setData(targetDiary);
            } else {
                alert('없는 일기입니다.');
                navigate('/', { replace: true });
            }
        }
    }, [id, diaryList, navigate]);

    if (!data) {
        return <div className="DiaryPage">로딩중 ...</div>;
    } else {
        const curEmotionData = emotionList.find((it) => parseInt(it.emotion_id) === parseInt(data.emotion));

        return (
            <div className="DiaryPage">
                <MyHeader
                    headText={`${getStringDate(new Date(data.date))} 기록`}
                    leftChild={<MyButton text={'뒤로가기'} onClick={() => navigate(-1)} />}
                    rightChild={<MyButton text={'수정하기'} onClick={() => navigate(`/edit/${data.id}`)}></MyButton>}
                />

                <article>
                    <section>
                        <h4>오늘의 감정</h4>
                        <div className={['diary_img_wrapper', `diary_img_wrapper${data.emotion}`].join(' ')}>
                            <img src={curEmotionData.emotion_img} />
                            <div className="emotion_descript" alt="">
                                {curEmotionData.emotion_descript}
                            </div>
                        </div>
                    </section>

                    <section>
                        <div className="diary_content_wrapper">
                            <h4>오늘의 일기</h4>
                            <div>
                                <p>{data.content}</p>
                            </div>
                        </div>
                    </section>
                </article>
            </div>
        );
    }
};

export default Diary;
```

1. URL 에서 Query 데이터를 받아오기 위한 useParams

```
const { id } = useParams();
```

2. 네비게이션을 사용하기 위한 선언

```
const navigate = useNavigate();
```

3. 일기 전체 데이터를 App에서 받아오기 위한 컨텐스트 선언

```
const diaryList = useContext(DiaryStateContext);
```

4. 다이어리 목록 상태 관리

```
const [data, setData] = useState();
```

5. 이전 문서와 동일하게 useEffect로 id값이나 다이어리 목록, 혹은 네비게이션이 달라졌을 때 실행 할 함수이다. 다이어리가 1개 이상일때 전체 목록에서 id값(URL로 넘어온 Query 값)이 같은 것만 뽑아내에서 data상태에 업데이트 시킨다. 없으면 홈으로 보낸 후 뒤로가기가 안되게 끔 막아 두었다.

```
useEffect(() => {
        if (diaryList.length >= 1) {
            const targetDiary = diaryList.find((it) => parseInt(it.id) === parseInt(id));

            if (targetDiary) {
                setData(targetDiary);
            } else {
                alert('없는 일기입니다.');
                navigate('/', { replace: true });
            }
        }
    }, [id, diaryList, navigate]);
```

6.  데이터가 없으면 로딩중이라는 div를 리턴하여 보여준다.

```
if (!data) {
        return <div className="DiaryPage">로딩중 ...</div>;
    }
```

7. emotionList는 감정에 따른 이미지, 아이디, 상태가 좋은지 나쁜지 등을 고 있는 배열 객체이다. 그 객체에서 지금 가지고 있는 일기 data와 비교하여 같은 감정인 객체를 뽑아내서 curEmotionData에 저장시켜둔다.
   우선 상단 부분은 MyHeader컴포넌트로 구현하였고 오늘의 감정 섹션은 curEmotionData에 있는 정보를 활용하여 이미지 src위치와 그 내용을 들고 와서 보여준다.  
   그리고 content부분은 전체에서 내용만 뽑아와서 보여주면 된다.
   이전 문서들을 이해했다면 전혀 어려울 것이 없는 부분이다.

```
else {
        const curEmotionData = emotionList.find((it) => parseInt(it.emotion_id) === parseInt(data.emotion));

        return (
            <div className="DiaryPage">
                <MyHeader
                    headText={`${getStringDate(new Date(data.date))} 기록`}
                    leftChild={<MyButton text={'뒤로가기'} onClick={() => navigate(-1)} />}
                    rightChild={<MyButton text={'수정하기'} onClick={() => navigate(`/edit/${data.id}`)}></MyButton>}
                />

                <article>
                    <section>
                        <h4>오늘의 감정</h4>
                        <div className={['diary_img_wrapper', `diary_img_wrapper${data.emotion}`].join(' ')}>
                            <img src={curEmotionData.emotion_img} />
                            <div className="emotion_descript" alt="">
                                {curEmotionData.emotion_descript}
                            </div>
                        </div>
                    </section>

                    <section>
                        <div className="diary_content_wrapper">
                            <h4>오늘의 일기</h4>
                            <div>
                                <p>{data.content}</p>
                            </div>
                        </div>
                    </section>
                </article>
            </div>
        );
    }
};
```
