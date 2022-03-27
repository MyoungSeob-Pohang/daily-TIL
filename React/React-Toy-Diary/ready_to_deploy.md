# 배포 준비하기

실질적으로 배포하기전에 마지막 테스트 과정을 거쳐야 한다. 일단 기본적인 html lang와 title, meta태그들을 배포에 맞게 수정하도록 하자.
public/index.html 파일에서 수정 할 수 있다.

```
<!DOCTYPE html>
<html lang="ko">

<head>
  <meta charset="utf-8" />
  <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <meta name="theme-color" content="#000000" />
  <meta name="description" content="나만의 감정 일기장" />
  <link rel="apple-touch-icon" href="%PUBLIC_URL%/logo192.png" />
  <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />

  <title>감정 일기장</title>
</head>

<body>
  <noscript>You need to enable JavaScript to run this app.</noscript>
  <div id="root"></div>
</body>

</html>
```

타이틀은 감정 일기장으로, description은 나만의 감정 일기장이라는 설명을 달아주었고 <html lang="ko">은 한국을 뜻하는 ko로 변경해 주었다.

리액트는 전체소스코드를 그대로 배포하면 띄워쓰기나 불필요한 공간도 많고 주요 파일들이 노출 될 수도 있기때문에 보안목적으로 혹은 성능적으로 빌드라는것을 따로 해주어야 한다. 명령어는 npm run build 이다. 명령어를 실행하면 build폴더가 생기고 그안에 배포에 필요한 파일들이 나타나게 된다. index.html을 보면 min형식으로 한줄 출력되는 걸 볼 수 있다. 그리고 서버에 배포해 보기위해 npm install -g serve 라는 패키지를 설치해주고 serve -s build를 눌러서 로컬로 접속을 해보면 된다. 에러가 뜬다면 그 에러를 수정 후 접속이 가능하다.  
지금 우리 프로젝트에서도 문제가 있는데 App컴포넌트의 useEffect 부분이다.

```
useEffect(() => {
        const localData = localStorage.getItem('diary');

        if (localData) {
            const diaryList = JSON.parse(localData).sort((a, b) => parseInt(b.id) - parseInt(a.id));

            dataId.current = parseInt(diaryList[0].id) + 1;
            dispatch({ type: 'INIT', data: diaryList });
        }
    }, []);
```

크게 문제없어 보이지만 로컬스토리지에 작성하거나 할때 모든 삭제가 지워진 상태에서 새로고침을 하면 diaryList[0] 이부분에서 문제가 발생한다. 실제 로컬스토리지에 보관된 값은 모두 삭제하여서 빈 배열만 가지고 있는데 0번째요소의 id에 접근하려고 하니 존재할리가 없다. 이 부분을 수정해야하는데 diaryList의 값이 1개 이상일 때 아이디 값과 디스패치 부분을 실행하면 된다.

```
useEffect(() => {
    const localData = localStorage.getItem('diary');

    if (localData) {
        const diaryList = JSON.parse(localData).sort((a, b) => parseInt(b.id) - parseInt(a.id));

        if (diaryList.length >= 1) {
            dataId.current = parseInt(diaryList[0].id) + 1;
            dispatch({ type: 'INIT', data: diaryList });
        }
    }
}, []);
```

그리고 페이지 마다 타이틀이 같은 것도 약간 보기 싫은데 각 컴포넌트 별로 혹은 타이틀을 바꾸고 싶은 컴포넌트별로 useEffect를 사용하여 title의 값을 변경시켜주면 된다.

```
useEffect(() => {
    const titleElement = document.getElementsByTagName('title')[0];
    titleElement.innerText = `감정 일기장 - ${id}번 일기`;
}, [id]);
```

이렇게 하면 배포준비가 끝이나게 된다. 실질적으로 수정사항이 생기면 다시 빌드해서 다시 serve -s build 를 실행해야 해서 아주 번거로운데, 배포과정에 문제가 생기면 안되니 마지막 테스트라 시작하고 열심히 테스트하자!
