# Why React.js

React는 왜 사용해야 하고, 왜 인기가 많은가 ?

일단 여러가지 이유가 있지만 내가 생각하는 React가 좋은 이유는 아래와 같다.

1. 중복된 코드를 완전히 줄여준다. 리액트는 컴포넌트로 재사용이 가능한 코드를 구현하기 때문에 어디서든 필요한 위치에 가져다 놓을 수 있고, 수정하기도 용이하다.
2. 명령형 프로그래밍이 아니라 선언형 프로그래밍이다. 명령형 : 절차를 하나하나 모두 나열 / 선언형 : 목적을 바로 말함.
3. 가상돔을 사용하기 떄문에 새로고침이 필요한 부분을 가상돔이 가지고 있어 바뀌는 부분만 교체해주면 되기 때문에 사용자 경험이 좋고 바뀌는 부분만 연산해 빠르다.
4. 커뮤니티가 굉장히 크고 많은 사람들이 사용하기 때문에 필요한 소스코드 혹은 정보가 굉장히 많이있고 구하기 쉽다.

물론 위 좋은이유가 단점이 되는 경우도 많이 존재한다. 특히 가상돔 부분은 아직도 논란이 많이있다. svelte와 비교가 많이 되는데 이 부분은 나중에 다른 문서에서 다루도록 하겠다.

## react App 빠르고 편하게 만들기 / create React App

react nodejs기반 자바스크립트 라이브러리이며 react에는 수많은 패키지들이 존재한다. 이런 패키지를 다운을 받자마자 바로 사용할 수 있는것이 아니라 여러가지 세팅을 해주어야 사용이 가능하다. 그래서 이런 세팅이 모두 되어있는 Boiler Plate(보일러 찍어내는 틀)이 존재한다.
사용하는 방법은 아주 간단한데, nodejs와 npm이 설치되어있으면 되고 터미널에서 **_npx create-react-app projectName_** 을 작성해주면 설치가 진행된다.  
시작은 **_ npm start _** 이다.

## JSX

```
const element = <h1>Hello, world!</h1>;
```

위와 같은 이상한 코드가 jsx문법이다. js같기도 하고 html 같기도한데, 둘이 합쳐놨다고 생각하면 편하다. 그래서 아래와 같은 코드도 사용이 가능하다.

```
function formatName(user) {
  return user.firstName + user.lastName;
}

const user = {
  firstName: 'moons',
  lastName: 'bi'
};

const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

element에 h1태그를 담고있고 h1태그에서 {}중괄호 안에 formatName함수를 호출하여 user안에 담긴 객체를 넘기고 있다. formatName함수는 user안에 내용을 받아서 성과 이름을 붙여서 리턴한다. 그 리턴된 값이 h1안에 담기게 되고 그 결과가 reactDOM에서 렌더링 되고 있다.

react에서는 js파일을 변환하여 사용한다. jsx로 사용하는 사람도 있는데 결국 jsx도 js로 변환되어 렌더링 되기 때문에 어떤것을 사용하던지 상관없다.  
js와 거의 유사하지만 html과 다른 점은 아래와 같다.

1. 닫는태그가 항상 있어야 한다. 닫는 태그가 없는태그(a, img 등)은 태그 끝에 **_ <img />_** 이렇게 끊어주면 된다.
2. 최상위 태그가 있어야 한다. 즉 다른 모든 태그를 감싸는 태그가 있어야 한다. 감싸는 태그가 없다면 만들어주거나, <React.Fragment></React.Fragment> 혹은 <></>태그로 감싸주면 된다.
3. css, js 모두 import를 사용한다.
4. class는 예약어이기 때문에 클래스명을 부여할때는 className을 사용한다.
5. html 안에서 값을 사용할 때에는 {} 중괄호 안에서 표현한다. 대신 숫자나 문자열만 표현할 수 있다.
