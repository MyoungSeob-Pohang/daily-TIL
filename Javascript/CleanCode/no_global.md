# 전역 공간 사용 최소화

전역공간은 왜 사용하면 안되는지 ? 혹은 최소화 해서 사용해야 하는 이유가 무었인지 알아보자.
전역공간을 최소화하라, 쓰지마라라는 말은 여러강의에서도 들어왔다. 하지만 이유가 무엇인지는 정확하게 개념을 잡고있지 않았다.

일단 전역공간은 무엇일까 ?  
전역공간을 window 혹은 global이라고 많이 부르는데 브라우저 환경에서 돌아가는 경우에는 window가 최상위이고 nodejs환경에서는 global이 최상위라 불린다. 하지만 크게 다른건 없다.

console로 window를 출력해보면 아주 많은 자바스크립트 API 명세들이 출력되는데, 흔히 말하는 window. 으로 시작되는 모든것들 예를 들면 alert, setTimeout, setInterval같은 모든 인터페이스들이 모두 만들어져 있다. 그냥 최상위 공간이 전역공간이다 라고 생각하면 된다.

예를 들어 생각해보자, 만약 index.html 에서 index.js 와 index2.js를 불러온다고 생각하자.  
그리고 index.js에서 var globalVar = 'global'; 이라 저장해 두었다고 치자. 그럼 globalVar변수는 window 즉 최상위에 저장이 되게 된다. 그래서 window.globalVar로도 출력이 가능하고 window를 콘솔에 찍어도 globalVar 값이 저장된 것을 볼 수있다. 자 그러면 index2.js에서 아무것도 없는데 console.log(globalVar); 을 출력하면 어떨까 ? 당연히 출력이 된다. 이렇게되면 전역공간을 사용하면 index.js와 index2.js의 코드가 겹칠수 도 있게된다. 파일을 나누면 코드 구역도 나눠진다 생각하는 사람이 많은데 그렇지 않다. 당연히 scope영역으로 나뉘게 된다.
