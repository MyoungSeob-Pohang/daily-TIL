# 불변성

불변성은 변하지 않는다와 같은말이다.

```
const originArray = ['123', '456', '789'];

const newArray = originArray;

originArray.push(10);
originArray.push(11);
originArray.push(12);
originArray.unshift(0);
```

위 코드의 대한 해답을 알고 있다면 불변성에 대해 알고 있는 것이다. originArray이라는 원본배열을 newArray에 복사해 두고 원본배열에 다른 값들을 추가하였다. 여기에서 원본배열과 new배열의 값은 ? originArray만 조작을 했는데도 그 결과가 newArray에도 동일하게 전달이 되었다. 얕은복사가 이루어졌기 때문인데 spread연산자를 사용했다면 괜찮지만 지금은 그냥 할당했기 때문에 문제가 생기는 것이다. const newArray = [...originArray] 로 사용했다면 이상이 없었다는 것이다.
위 코드는 2가지 방법으로 불변성을 지킬 수 있다.

1. 배열을 복사한다.
2. 새로운 배열을 반환하는 메서드들을 활용한다.

그럼 새로운 배열을 반환하는 메서드들이 어떤것들이 있을까 ? 많은 것들이 있지만 대표적으로 filter, map, slice 등이 있다.
