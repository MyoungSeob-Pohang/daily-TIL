# 버그 수정

전체 코드중에 버그를 일으키는 코드가 있다. 하나하나 무엇인지 알아보고 수정하자.

1. 2개 이상의 일기를 작성 하면 Encountered two children with the same key , `1` .... 오류

이 부분은 두개의 자식이 똑같은 Key값을 가지고 있다라는 버그다. 이 버그가 생긴이유는 현재 더미데이터 일기가 1~5번까지 지정이 되어있는데,  
App컴포넌트에서 key 값으로 사용하는 useRef의 초기값이 0이기 때문에 생기는 문제이다. 첫번째 일기를 만들면 0이 key값이지만 두번째 새 일기를 작성하여 key값이 1이 된다. 그러면 더미데이터가 가진 key값과 중복이 되기 때문에 이런 오류가 발생하는 것이다.  
고치는 방법은 간단하다. useRef의 값을 6부터 지정해주면 해결 된다.

2. 마지막 날짜로 일기를 작성하게 되면 새로 작성된 일기가 보이지 않는다. 문제가 있는 거 같아 components를 확인하니 데이터는 정상적으로 들어가고 있다. 왜 그런걸까 ?  
   날짜에 맞는 일기데이터를 추리는 과정에서 App 컴포넌트에서 첫째날과 마지막날짜를 가져오는 코드가 있는데, 아래 코드이다.

```
const firstDay = new Date(curDate.getFullYear(), curDate.getMonth(), 1).getTime();
const lastDay = new Date(curDate.getFullYear(), curDate.getMonth() + 1, 0).getTime();
```

이 부분에서 lastDay가 문제가 있다. 지금 보면 마지막 날짜만 입력을 해 두었는데, 실제로 new Date객체는 시간 , 분, 초까지 입력을 받을 수 있다. 지금은 0시 0분 0초로 마지막 날을 지정하고 있는데 그렇게 되면 마지막 날 0시 0분 0초가 넘어가는 모든 일기는 목록에서 제외된다. 그래서 시 분 초 까지 모두 입력해 두어야 한다.

```
onst lastDay = new Date(curDate.getFullYear(), curDate.getMonth() + 1, 0, 23, 59, 59).getTime();
```

이러면 문제 없이 모든 일기를 확인할 수 있다. 23시 59분 59초로 지정 해 두었다.
