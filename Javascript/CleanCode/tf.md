# Truthy & Falsy

[Truthy & Falsy](https://github.com/MyoungSeob-Pohang/daily-TIL/blob/main/Javascript/JS-%EC%9D%91%EC%9A%A9/truthy_falsy.md)에서도 한번 다룬 적이 있는 섹션이다.

자바스크립트는 아무래도 동적인 타입비교를 가진 언어이다 보니 형변환이 아주 자유롭다. 심지어 사용자가 의도하지 않아도 형 변환이 일어나게 된다.

```
if ('string'.length > 0){}
if (!isNaN(10)){}
if (boolean === true){}
```

위 코드를 보면 참 이상한거 같다. 이미 Truthy & Falsy의 개념을 알고나니 저런식으로 코드를 짤 수 있구나라는 생각조차 든다. 실제로 Truthy & Falsy개념을 안다면 아래와 같은 코드로 변환할 것이다.

```
if ('string'.length){}
if (10){}
if (boolean){}
```

두 코드는 완전히 같다. Truthy이기 때문이다. if문안에 모든 값이 바로 true로 귀결된다. 참 같은 값이라 해서 아래와 같은 값들이 있다.

```
if (true)
if ({})
if ([])
if (42)
if ("0")
if ("false")
if (new Date())
if (-42)
if (12n)
if (3.14)
if (-3.14)
if (Infinity)
if (-Infinity)
```

위와 같은 값은 참같은 값이라고 해서 true 값을 가진다. 거짓같은 값은 아래와 같다.

```
if (false)
if (null)
if (undefined)
if (0)
if (-0)
if (0n)
if (NaN)
if ("")
```

위 값들은 거짓 즉 false와 같은 값을 가진다. 실제로 이 값들은 유용하게 사용될 수 있는데,

```
function printName(name) {
    if(name === undefined || name === null) {
        return '사람 없음';
    }
    return '안녕하세요' + name + '님';
}
```

name값이 비엇을 경우를 대비하여 undefined 또는 null이 아닌지 확인을 하고있는데 여기에서 not name만 넘겨줘도 된다. 왜냐하면 Falsy가 가지는 값 중에는 null과 undefined가 포함되 있기 때문이다.

```
function printName(name) {
    if(!name) {
        return '사람 없음';
    }
    return '안녕하세요' + name + '님';
}
```

이렇게 코드를 작성하면 문자열이 입력되어 있다면 Truthy가 될 것이고 null이나 undefined이면 Falsy가 되면서 자동으로 if문 안에 조건을 실행하게 된다.
