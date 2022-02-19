# Object freeze

Object freeze라는 말을 처음 들어 봤다. 검색을 해보니 객체를 동결시킨다고 한다. 동결된 객체는 더 이상 변경될 수 없게 변해서 추가하거나 제거하는 것을 방지한다고 한다.

```
const obj = {
  prop: 42
};

Object.freeze(obj);

obj.prop = 33;
// Throws an error in strict mode

console.log(obj.prop);
// expected output: 42
```

정말 동결이 되었는지 확인할 때는 Object.isFrozen()를 사용하면 true/false 값으로 동결이 되었는지 안되었는지 알 수 있다. 정말 유용할것 같다.  
하지만 여기서도 문제가 하나 있는데 객체 안에 또 다른 객체가 존재한다고 하면 Object freeze도 깊은 복사에는 관여할 수 없고 얕은 복사를 하기 때문에 객체 안에 객체의 값은 추가, 삭제, 변경이 가능하다. 그렇다면 이 부분은 어떻게 해결해야 될까 ?

1. 대중적인 유틸 라이브러리 사용 (lodash)
2. 직업 유틸 함수 생성
3. stackoverflow 같은 커뮤니티에서 코드를 보고 읽고 괜찮은 코드 사용
4. typescript 사용 ( readonly )
