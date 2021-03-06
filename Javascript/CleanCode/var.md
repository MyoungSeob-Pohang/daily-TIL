# var를 지양하자

자바스크립트를 공부할 때 var말고 let이나 const를 사용하지마라는 말을 많이 들었다.  
보통 자바스크립트 책이나 강의를 할때 변수부터 배우게 되는데 const나 let을 사용하라는 이야기가 많다.  
const나 let은 ES2015부터 나오게 되어 그 이전에는 var를 사용하였고 자바스크립트가 발전하면서 let & const가 더 좋은 기능을 가지게 된 것이다.

큰 차이점은 var는 함수스코프, const & let은 블록 단위스코프를 가지게 된다.

```
var name = '이름';
var name = '이름2';
```

지금보면 var로 동일한 변수이름으로 다른 값을 담아두고 있다. 그런데 에러가 나지 않는다. 열번 넘개 선언하여도 똑같다.
호출하면 가장 마지막에 선언된 name이 나오게 된다. 어찌보면 너무 편하게 사용할 수 있다고 생각이 되는데, 생각해보면 너무 위험하다.
코드의 길이가 길어졌을 때 만약 1000Line이라고 했을 때 var로 선언하게 되면 name이 무슨값을 가지고 있는지 찾아봐야 한다. var name변수가 어떤 함수안에서 값이 바뀔 수도 있고 다른 부분에서 값이 바뀔수 있는데 그 로직들을 다 따라가면서 무슨 값이 나오는지 찾아야 하는 것이다.

하지만 const & let로 바꾸면 어떻게 될까 ?

```
let name = '이름';
let name = '이름2'; // error
```

let은 var와 다르게 같은 변수명을 사용할 수 없다. 그렇기 때문에 무슨 값을 가지고 있는지 한눈에 알아볼 수 있고, 만약 재 할당을 하게 된다 하여도 var보다 알아보기가 훨씬 편하다.

const는 let과 무엇이 다를까 ?

```
const name = '이름';
const name = '이름2'; // error
```

에러가 나는 부분은 똑같다. 하지만 let은 재할당이 가능하지만 const는 재할당이 불가능하다. const는 한번 선언하게 되면 값을 바꿀 수 없다. 그리고 선언할 때 무조건 값이 지정되어야 한다. 그렇지 않으면 에러가 발생한다. 재할당이 불가능 하기 때문이다.

```
let name;
name = '이름';
name = '이름1';
```

```
const name; // error
const name = '이름';
name = '이름2'; // error
```
