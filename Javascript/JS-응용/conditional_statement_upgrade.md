# 조건문 업그레이드

배열의 내장함수를 사용하면 조건문을 조금 더 멋지게 쓸 수 있다.  
만약 음식을 입력받아서 그 음식이 한국음식인지 확인하는 함수가 있다고 가정하자. 그럼 아래 코드와 같이 작성할 수 있다.

```
function isKoreanFood(food) {
  if (food === "불고기" || food === "비빔밥" || food === "떡볶이") {
    return true;
  } else {
    return false;
  }
}

const food1 = isKoreanFood("불고기");
const food2 = isKoreanFood("파스타");

console.log(food1); // true
console.log(food2); // false
```

하지만 모든 한식을 검사해야 한다고 하면 if문이 굉장히 길어지거나 switch문을 쓰더라도 case가 굉장히 많이 나올 것이다.
그럼 어떻게 해야할까 ? food라는 배열을 하나 선언 후 입력해 두고 에 입력받은 음식이 배열안에 존재하는지 안하는지 확인하면 될거같다.

```
const koreanFood = ["불고기", "비빔밥", "떡볶이", "김치"];

function isKoreanFood(food) {
  if (koreanFood.includes(food)) {
    return true;
  } else {
    return false;
  }
}

const food1 = isKoreanFood("불고기");
const food2 = isKoreanFood("파스타");
const food3 = isKoreanFood("김치");

console.log(food1); // true
console.log(food2); // false
console.log(food3); // true
```

이렇게 배열의 내장함수인 includes 사용하여 조금 더 편하고 간결한 코드를 작성할 수 있게 된다.

한가지 다른 예제를 보자, 이번 예제는 국가를 한식인지 중식인지 일식인지 양식인지 에 따라서 대표메뉴를 return 하는 함수가 있다.

```
const getMeal = (mealType) => {
  if (mealType === "한식") return "김치";
  if (mealType === "양식") return "파스타";
  if (mealType === "중식") return "멘보샤";
  if (mealType === "일식") return "초밥";

  return "굶기";
};

console.log(getMeal("한식")); // 김치
console.log(getMeal("양식")); // 파스타
console.log(getMeal()); // 굶기
```

지금은 4개 정도라 괜찮지만 만약 식사유형이 4가지 말고 10가지가 더 추가 된다면 어떨까 ? 코드가 길어지고 관리하기 어려울 듯 하다.
이럴때는 괄호표기법으로 해결할 수 있다.

```
const meal = {
  한식: "김치",
  중식: "멘보샤",
  일식: "초밥",
  양식: "파스타",
  인도식: "카레",
  미국식: "햄버거"
};

const getMeal = (mealType) => {
  return meal[mealType] || "굶기";
};

console.log(getMeal("한식"));
console.log(getMeal("미국식"));
console.log(getMeal());
```

차례대로 코드를 해석해보자. 일단 meal라는 상수에 각 식사유형에 따른 대표메뉴를 선언하였다. 그리고 getMeal 함수에서 mealType를 받아 meal객체의 괄호표기법으로 접근하였다.
그래서 정상적인 코드로 동작하게 된다. 위 작성코드보다 조금 더 유연하고 관리하기가 용이해졌다.
