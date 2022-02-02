# Props / 컴포넌트에 데이터를 전달하는 방법

State와 같이 react에서 아주 중요한 역할이다.

state에서 사용한 예제를 다시보자.

```
App.js

import React from 'react';
import Count from './count';

function App() {
       return (
        <Container>
            <div>
                <Header />
                <Count />
            </div>
        </Container>
    );
}

export default App;
```

```
count.js

import React, { useState } from 'react';

const Count = () => {
    const [count, setCount] = useState(0);

    const add = () => {
        setCount(count + 1);
    };

    const minus = () => {
        setCount(count - 1);
    };

    return (
        <div>
            <h2>{count}</h2>
            <button onClick={add}>+</button>
            <button onClick={minus}>-</button>
        </div>
    );
};

export default Count;
```

자, 근데 만약에 App컴포넌트에서 Count컴포넌트에게 count의 초기값을 0이 아닌 App컴포넌트가 전달하는 값을 사용해라 라고 한다면 어떻게 해야할까 ?  
이럴때 Props이라는 기능을 사용하면 되는데 , <Count />를 호출할때 <Count initialValue={5} /> 이런식으로 자식 컴포넌트에게 initialValue 라는 이름을 붙이고 {5}라는 값을 넘겨줄 수 있다. 부모인 App컴포넌트에서 자식Count컴포넌트에게 어떠한 이름을 붙이고 값을 전달하는 행위를 Props라고 부르고, 저런 값을 복수형으로 보내면 Props라고 한다.  
전달한 initialValue={5}를 count컴포넌트에서는 매개변수로 받아서 사용할 수 있는데,

```
import React, { useState } from 'react';

const Count = (props) => {
    console.log(props);

    const [count, setCount] = useState(0);

    const add = () => {
        setCount(count + 1);
    };

    const minus = () => {
        setCount(count - 1);
    };

    return (
        <div>
            <h2>{count}</h2>
            <button onClick={add}>+</button>
            <button onClick={minus}>-</button>
        </div>
    );
};

export default Count;

출력 : {initialValue : 5}
```

위 출력처럼 값이 props에 담기고 객체형태로 출력된다. 그럼 당연히 전달할때 여러값을 넘길 수도 있다. <Count a={10} initialValue={5} /> 이런식으로 여러 값을 보낼 수 있다.
값은 동일하게 출력하게 되고 객체안에 여러개의 값으로 표현이 된다. 그래서 넘길때 자식컴포넌트에 나열해서 보내는게 아니라 따로 객체를 만들어서 값을 보낸다.

```
App.js

    const counterProps = {
        a: 10,
        b: 20,
        c: 30,
        d: 40,
        e: 50,
        initialValue: 5,
    }

    <Count {...counterProps} />
```

```
count.js

const Count = ({ value }) => {
    const [count, setCount] = useState(value);

    const add = () => {
        setCount(count + 1);
    };

    const minus = () => {
        setCount(count - 1);
    };

    return (
        <div>
            <h2>{count}</h2>
            <button onClick={add}>+</button>
            <button onClick={minus}>-</button>
            <OddEvenResult count={count} />
        </div>
    );
};
```

app에서 객체로 값을 넘겨준 것을 { value }로 받아서 사용하였다. 근데 만약 넘길때 값을 빠트렸다면 초기값을 정해줄 수도 있는데 아래 코드와 같다.

```
import React, { useState } from 'react';

const Count = ({ value }) => {
    const [count, setCount] = useState(value);

    const add = () => {
        setCount(count + 1);
    };

    const minus = () => {
        setCount(count - 1);
    };

    return (
        <div>
            <h2>{count}</h2>
            <button onClick={add}>+</button>
            <button onClick={minus}>-</button>
        </div>
    );
};

Count.defaultProps = {
    value: 3,
};

export default Count;
```

```
Count.defaultProps = {
    value: 3,
};
```

위 코드처럼 defaultProps를 사용하면 App.js에서 넘어온 값이 없을때 초기값으로 value를 지정해줄 수 있다.
