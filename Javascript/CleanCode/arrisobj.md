# JavaScript의 배열은 객체다.

```
const arr = [1, 2, 3];

arr[3] = 'test';
arr['obj'] = {};
arr[{}] = [1, 2, 3];
arr['property'] = 'string value';
arr['func'] = function(){
    return 'hello';
}

for(let i = 0; i < arr.length; i++) {
    console.log(arr[i])
}
```

위 코드의 결과는 어떻게 나올까 ? 결과는 1, 2, 3, test 이렇게만 나온다. 이상하지 않는가 ? 모두가 arr에 들어가는것이 아닐까 ?  
실제로 값이 들어가지만 for 문안에 i값이 숫자 값이라 test 까지의 값은 찍히지만 나머지 값이 찍히지 않는다. for문을 제외하고 arr만 찍어보면 나머지 값이 다 정상적으로 들어간 것을 알 수있다.  
하지만 값이 배열에 들어갔다고 해도 이상하다. 모두가 들어간 것도 이상하지 않을까 ? arr['property'] = 'string value'; 배열에 인덱스에 string을 넣고 문자열을 넣으니 그것도 들어가고 arr['obj'] = {}; 빈 객체도 들어가고 심지어 arr[{}] = [1, 2, 3]; 객체를 넣었더니 [object object] : [1, 2, 3]값으로 형변환 되서 문자열로 바뀌어서 들어갔다. 이게 배열이라고 할 수 있을까 ? 자바스크립트의 객체와 완전 동일한 역할을 하고있다.

```
const obj = {
    arr: [1, 2, 3],
    3 : 'test',
    property : 'string value',
    obj : {},
    '{}' : [1, 2, 3],
    func : function () {
        return 'hello'
    }
}
```

이 객체와 위 코드의 배열은 거의 모든 부분이 유사하다. 여기서 배열과 객체가 아주 유사하다는 것을 알 수 있고 결국 자바스크립트의 배열은 객체다 라는 것을 알 수 있다.  
자바스크립트 배열의 내부 동작은 객체와 비슷한 부분이 아주 많은 것이다. 그래서 배열을 다룰때는 주의사항이 필요한데, 결국은 객체스러운 면이 많고 객체처럼 취급될 떄가 있기 때문이다.

배열이 진짜 배열인지 확인할 때에는 아래와 같은 코드를 사용하자.

```
const arr [1, 2, 3];

Array.isArray(arr);
```

Array.isArray를 사용하면 배열인지 아닌지 확인이 가능하다. 결과값은 true, false로 나온다.
