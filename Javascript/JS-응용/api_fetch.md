# API & Fetch

# API

API : 응용 프로그램 프로그래밍 인터페이스의 약자로 응용 프로그램에서 사용할 수 있도록 운영체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스를 뜻한다.
주로 파일 제어, 창 제어, 화상 처리, 문자 제어 등을 위한 인터페이스를 제공한다. - WIKI 백과

설명이 너무나 어렵다. 쉽게 설명이 필요한거 같다.  
레스토랑을 생각해보자, 이 레스토랑은 방문하는 손님이 음식을 먹으려면 4가지의 단계를 거친다.

1. 손님이 웨이터에게 음식을 주문한다.
2. 주문을 받은 웨이터가 냉장고에서 음식을 찾는다.
3. 그리고 웨이터가 그 음식을 가져와서 조리를하고,
4. 손님에게 음식을 서빙한다.

위 4가지 과정이 우리가 웹사이트에서 어떠한 정보를 보는방식과 매우 유사하다. 실제로 우리가 웹사이트에서 정보를 보는 방식은

1. 브라우저(클라이언트)가 서버에 Request를 요청한다 ( 주문 )
2. 서버가 Database에 Query를 날린다. ( 찾기 )
3. Database에서 가져온 data를 가공하여 Query Result를 받아서 (가져오기 , 조리)
4. 브라우저(클라이언트)에게 Response(서빙)을 한다.

조금더 쉽게 풀이하면 아래와 같다.

1. 웹브라우저 즉 손님은 음식을 요청하는거 처럼 웹브라우저도 데이터를 요청(Request)한다. 그래서 그 웹브라우저를 클라이언트라고 할 수 있다.
2. 주문(Request)을 웨이터(Server)가 받는다.
3. 웨이터가 냉장고(Database)에 가서 그 주문한 음식이 있는지 확인한다. (Query).
4. 냉장고에서 주문 받은 음식을 꺼내서(Query Result)
5. 웨이터가 손님에게 음식을 전달해준다.(Response)

우리가 자바스크립트로 개발하는 부분은 위 과정 중 손님인 클라이언트 부분이다.  
그래서 이 클라이언트 관점에서 데이터요청(Request)과 데이터전달(Response)과정만 알면된다. 이렇게 웹브라우저 클라이언트를 통해서 서버에게 데이터를 요청하고 전달받는 과정을 API호출이라고 한다. 즉 API는 클라이언트와 서버처럼 프로그램과 프로그램의 연결다리이자 대화를 할 수 있는 방법 정도로 이해하면 된다.

API호출은 어떤 데이터를 받기위한 목적으로 함수를 호출하는것과 비슷한데, 함수도 호출해서 원하는 데이터를 받기위해서 사용하기 때문이다.
하지만 함수와 가장 큰 차이점은 우리가 이 응답을 도대체 언제 받을지 확실히 알 수 없다는 것이다.  
보통 클라이언트와 서버의 컴퓨터는 다른 컴퓨터이고, 클라이언트가 요청했을 때 다른 컴퓨터에 있는 서버가 응답을 할 때까지는 시간은 인터넷연결속도, 서버의 부하 등에 의해 예상할 수 없고 때로는 완전히 실패해버리기도 한다. 그래서 우리가 Promise객체에 reject 상태가 있는 것이다.  
언제 끝날지도 모르는 작업을 모두 동기적으로 작업할 수 없다. 그래서 이런 API호출에는 Promise객체를 이용하여 비동기 호출을 하게된다.

지금부터 특정서버에 데이터를 호출하고 그 데이터를 받아보는 API 호출코드를 알아보자.  
API는 그냥 할 수 없고 누군가에게 말을 걸어야 하는데, 아무한테나 말을 건다고 대답을 해주는 것이 아니고 인증된 누군가에 한해서만 서버가 요청에 대답을 하는 경우가 대다수이다.
구글에 JsonPlaceHolder이라는 Site가 있다. 이 사이트에 가보면 Resources 라는 항목에 개발자들에게 무료로 API호출에 대해서 더미데이터를 응답해주는 서비스를 해준다.
이런 경우 OpenAPI를 제공한다라고 한다.

[JsonPlaceHolder](https://jsonplaceholder.typicode.com/)

위 사이트 Resources에서 /posts 눌러보면 JSON데이터 들이 여러개가 나오고 그 사이트 주소가 우리가 API호출할 때 누군가에게 말을 걸 것인지 결정하는 주소이다.

[JsonPlaceHolder-posts](https://jsonplaceholder.typicode.com/posts)

```
let response = fetch("https://jsonplaceholder.typicode.com/posts");

response.then((res) => {
  console.log(res);
});

출력 : Response {type: "cors", url: "https://jsonplaceholder.typicode.com/posts", redirected: false, status: 200, ok: true…}
        type: "cors"
        url: "https://jsonplaceholder.typicode.com/posts"
        redirected: false
        status: 200
        ok: true
        statusText: ""
        headers: Headers
        body: ReadableStream
        bodyUsed: false
        arrayBuffer: ƒ arrayBuffer() {}
        blob: ƒ blob() {}
        clone: ƒ clone() {}
        formData: ƒ formData() {}
        json: ƒ json() {}
        text: ƒ text() {}
        <constructor>: "Response"
```

위 코드에서 fetch를 사용하는데 이 fetch내장함수는 자바스크립트에서 API호출을 할 수 있도록 도와주는 내장함수이다. 위 코드에서 response에 담기는 값은 반환은 Promise형태이고, 이것을 그대로 출력하게되면 그 API 성공 객체자체를 반환하기 때문에 출력이 위와같이 나온다. 즉 편지에 대한 답장을 받았는데 편지봉투를 안뜯고 편지봉투를 출력한 상황이다 라고 이해하면 된다. 그럼 편지의 내용은 어떻게 확인할까 ?

```
async function getData() {
  let rawResponse = await fetch("https://jsonplaceholder.typicode.com/posts");
  let jsonResponse = await rawResponse.json();

  console.log(jsonResponse);
}

getData();

출력 : (100) [Object, Object, Object, Object, Object, Object, Object, Object, Object, Object, …]
```

위 코드에서 getData함수를 작성하여 await로 data를 가지고 오고 그 데이터를 json형태로 파싱 해주었다. 그리고 그 값을 출력하였다.
**_ 파싱 : 파싱은 어떤 페이지(문서, html 등)에서 내가 원하는 데이터를 특정 패턴이나 순서로 추출해 가공하는 것 _**
