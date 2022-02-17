# 유사 배열 객체

앞서 문서에서 자바스크립트의 배열은 객체다라고 했다.

```
const arrayLikeObject = {
    0: 'hello',
    1: 'world',
    length: 2,
};
```

arrayLikeObject라는 객체를 만들었고 key값에 인덱스를 넣어두었고 쌩뚱맞게 length라는 프로퍼티를 만들었다. 여기서 Array.from()이라는 메서드를 활용하면 저 객체를 배열로 만들 수가 있는데, 그럼 위 객체는 바꿧을 때 어떻게 보일까 ? 완전한 객체로 바뀔 수 있을까 ?

```
const arrayLikeObject = {
    0: 'hello',
    1: 'world',
    length: 2,
};

const arr = Array.from(arrayLikeObject);

console.log(arr); //  ["hello", "world"]
console.log(Array.isArray(arr)); // true
console.log(Array.isArray(arrayLikeObject));  // false
```

결과를 보면 ["hello", "world"] 가 잘 출력이 된 것을 볼 수 있고, arr은 배열인 것을 결과를 통해 알 수 있다. 자 값은 정확하게 들어갔는데 특이하고 웃긴건 length값도 잘 들어 갔다는 것이다.

```
console.log(arr.length); // 2
```

조금 혼란한데, 자바스크립트의 배열은 객체다 라는 것을 잊으면 안된다.  
실제로 유사배열 객체는 다양한 사례에서 만나볼 수 있는데, 그 중 대표적인게 argument 아규먼트 이다. 아규먼트는 가변적으로 변하는 인자들을 넘길 때 매개변수를 선언하지 않았음에도 함수내부에서 argument라는 유사배열 객체로 다룰 수가 있다.

```
function generatePriceList() {
    for(let i = 0; i < arguments.length; i++) {
        const element = argument[i];

        console.log(element)
    }
}

generatePriceList(100, 200, 300, 400, 500, 600);
```

보시다시피 generatePriceList함수에 매개변수가 없는데도 불구하고 arguments로 다양한 인자의 모든 값에 접근하고 있다. for문안에서 동작했기 떄문에 배열이구나 생각할 수 있지만 실제로는 유사배열 객체인 것이다. 실제로 Array.isArray로 확인해보면 false가 나온다. loop를 돌릴 수 있다는 이유로 배열로 착각하는 사람이 많은데, 실제로는 유사배열객체인 것이다. 그렇기 때문에 map과 같은 고차함수도 사용할 수 없다. 사용하려고 하면 배열로 형변환을 해주어야 한다(Array.from)
