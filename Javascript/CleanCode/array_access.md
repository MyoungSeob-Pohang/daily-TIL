# 배열 요소에 접근하기

배열요소에 접근하는 방법은 여러가지가 있다. 많이 쓰는 코드 중에 아래와 같은 코드들이 있다.

```
function clickGroupButton() {
    const confirmButton - document.getElementsByTagName('button')[0]
    const cancelButton - document.getElementsByTagName('button')[1]
    const resetButton - document.getElementsByTagName('button')[2]
}
```

위와 같은 코드를 비슷하거나 동일하게 많이 사용할 것이다. 하지만 실제로 [0] [1] [2] 로 지정된 것들이 무엇인지 알 수 없다는 문제가 존재한다.  
실제로 외국에서든 국내에서든 split를 사용할 때 가장많이 발생하는데, split을 사용하면 문자열을 특정한 기준으로 자르고 그 값들을 배열에 담아서 리턴한다.
그만큼 직관적이지 않다는 것인데 여러가지 방법을 통해서 변경할 수 있다.

1. 배열 구조 분해 할당

```
const [confirmButton, cancelButton, resetButton] = document.getElementsByTagName('button');
```

굉장히 명시적으로 보여지고 코드도 간단해졌다.

2. 유틸함수를 만든다. 아래 코드는 배열의 첫번째 값을 가져오는 코드이다.

```
function head(arr) {
    return arr[0] ?? '';
}
```

결론적으로 배열에 접근할때 암호쓰듯이 [0], [1] 이런 값에 접근하는 방법을 최대한 지양하고 조금 더 명시적으로 표현할 수 있는 방법을 찾는게 좋다.
