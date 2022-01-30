# spread

배열과 객체를 한줄로 펼치는 방법이라고 spread를 소개할 수 있다. 자 예를 들어 3가지 맛 쿠키를 객체에 담는다고 가정하자.

```
const cookie = {
  base: "cookie",
  made: "korea",
};

const chocoCookie = {
  base: "cookie",
  made: "korea",
  topping: "choco",
}

const bananaCookie = {
  base: "cookie",
  made: "korea",
  topping: "banana",
}
```

위 객체를 보면 base, made가 중복되는 것을 볼 수 있다. 만약 쿠키가 계속 추가 되어야 한다면 base, made는 계속 동일하게 코드가 사용될 것이다.
이렇게 만약 중복된 프로퍼티를 계속 작성해야 하는 상황이 오면 spread를 사용하는게 훨씬 쉽고 간단하다.
spread는 확산, 퍼짐, 전개 등의 뜻이 있는데 그냥 말그대로 퍼진다 라고 생각하면 된다. spread를 사용하면 아래와 같이 코드를 작성할 수 있다.

```
const cookie = {
  base: "cookie",
  made: "korea",
};

const chocoCookie = {
  ...cookie,
  topping: "choco",
}

const bananaCookie = {
  ...cookie,
  topping: "banana",
}

console.log(chocoCookie);
console.log(bananaCookie);

출력 : {base: "cookie", made: "korea", topping: "choco"}
        base: "cookie"
        made: "korea"
        topping: "choco"

        {base: "cookie", made: "korea", topping: "banana"}
        base: "cookie"
        made: "korea"
        topping: "banana"
```

...cookie 이게 바로 spread의 사용법이다. cookie객체에 담긴 base와 made가 chocoCookie, bananaCookie에 모두 포함이 된다.
자 이 spread는 객체에서만 사용이 가능할까 ? 아니다 배열도 동일하게 사용가능하다.

```
const noTopingCookies = ["촉촉한쿠키", "퍽퍽한쿠키"];
const TopingCookies = ["초코쿠키", "바나나쿠키", "딸기쿠키"];

const allCookies = [...noTopingCookies, "새로 추가된 쿠키", ...TopingCookies];

console.log(allCookies);

출력 : (6) ["촉촉한쿠키", "퍽퍽한쿠키", "새로 추가된 쿠키", "초코쿠키", "바나나쿠키", "딸기쿠키"]
        0: "촉촉한쿠키"
        1: "퍽퍽한쿠키"
        2: "새로 추가된 쿠키"
        3: "초코쿠키"
        4: "바나나쿠키"
        5: "딸기쿠키"
```

배열도 객체와 마찬가지로 사용할 수 있고, spread로 펼치기 전에 새로 추가된 쿠키 처럼 새로운 요소를 추가할 수도 있다.
