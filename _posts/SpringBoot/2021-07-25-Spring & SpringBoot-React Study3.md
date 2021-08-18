### 컴포넌트의 라이프사이클 메서드

모든 리액트 컴포넌트에는 라이프사이클(수명 주기)이 존재합니다. 컴포넌트의 수명은 페이지에 렌더링되기 전인 준비 과정에서 시작하여 페이지에서 사라질 때 끝납니다.



리액트 프로젝트를 진행하다 보면 가끔 컴포넌트를 처음으로 렌더링할 때 어떤 작업을 처리해야 하거나 컴포넌트를 업데이트하기 전후로 어떤 작업을 처리해야 할 수도 있고, 또 불필요한 업데이트를 방지해야 할 수도 있습니다.

이때는 컴포넌트의 라이프사이클 메서드를 사용합니다. 참고로 라이프사이클 메서드는 클래스형 컴포넌트에서만 사용할 수 있습니다. 함수형 컴포넌트에서는 사용할 수 없는데요. 그 대신에 Hooks 기능을 사용하여 비슷한 작업을 처리할 수 있습니다.

**Will** 접두사가 붙은 메서드는 어떤 작업을 작동하기 **전**에 실행되는 메서드이고, **Did** 접두사가 붙은 메서드는 어떤 작업을 작동한 **후**에



**마운트**

DOM이 생성되고 웹 브라우저상에 나타나는 것을 마운트(mount)라고 합니다. 이때 호출하는 메서드는 다음과 같습니다.

• constructor: 컴포넌트를 새로 만들 때마다 호출되는 클래스 생성자 메서드입니다.

• getDerivedStateFromProps: props에 있는 값을 state에 넣을 때 사용하는 메서드입니다.

• render: 우리가 준비한 UI를 렌더링하는 메서드입니다.

• componentDidMount: 컴포넌트가 웹 브라우저상에 나타난 후 호출하는 메서드입니다



**업데이트**

컴포넌트는 다음과 같은 총 네 가지 경우에 업데이트합니다.

**1.** props가 바뀔 때

**2.** state가 바뀔 때

**3.** 부모 컴포넌트가 리렌더링될 때

**4.** this.forceUpdate로 강제로 렌더링을 트리거할 때



getDerivedStateFromProps

shouldComponentUpdate

render

getSnapshotBeforeUpdate

componentDidUpdate



**언마운트**

마운트의 반대 과정, 즉 컴포넌트를 DOM에서 제거하는 것을 언마운트(unmount)라고 합니다.

• componentWillUnmount: 컴포넌트가 웹 브라우저상에서 사라지기 전에 호출하는 메서드입니다.



```
render() { ... }
```

이 메서드는 컴포넌트 모양새를 정의합니다. 그렇기에 컴포넌트에서 가장 중요한 메서드라고 할 수 있죠. 라이프사이클 메서드 중 유일한 필수 메서드이기도 합니다.

이 메서드 안에서 this.props와 this.state에 접근할 수 있으며, 리액트 요소를 반환합니다. 요소는 div 같은 태그가 될 수도 있고, 따로 선언한 컴포넌트가 될 수도 있습니다. 아무것도 보여 주고 싶지 않다면 null 값이나 false 값을 반환하도록 하세요.



이 메서드 안에서는 이벤트 설정이 아닌 곳에서 setState를 사용하면 안 되며, 브라우저의 DOM에 접근해서도 안 됩니다. DOM 정보를 가져오거나 state에 변화를 줄 때는 componentDidMount에서 처리해야 합니다.



```
constructor(props) { ... }
```

이것은 컴포넌트의 생성자 메서드로 컴포넌트를 만들 때 처음으로 실행됩니다. 이 메서드에서는 초기 state를 정할 수 있습니다.



```
static getDerivedStateFromProps(nextProps, prevState) {
    if(nextProps.value != = prevState.value) { // 조건에 따라 특정 값 동기화
      return { value: nextProps.value };
    }
    return null; // state를 변경할 필요가 없다면 null을 반환
}
```

props로 받아 온 값을 state에 동기화시키는 용도로 사용하며, 컴포넌트가 마운트될 때와 업데이트될 때 호출됩니다.



```
componentDidMount() { ... }
```

이것은 컴포넌트를 만들고, 첫 렌더링을 다 마친 후 실행합니다. 이 안에서 다른 자바스크립트 라이브러리 또는 프레임워크의 함수를 호출하거나 이벤트 등록, setTimeout, setInterval, 네트워크 요청 같은 비동기 작업을 처리하면 됩니다.



```
shouldComponentUpdate(nextProps, nextState) { ... }
```

이것은 props 또는 state를 변경했을 때, 리렌더링을 시작할지 여부를 지정하는 메서드입니다. 이 메서드에서는 반드시 true 값 또는 false 값을 반환해야 합니다. 컴포넌트를 만들 때 이 메서드를 따로 생성하지 않으면 기본적으로 언제나 true 값을 반환합니다. 이 메서드가 false 값을 반환한다면 업데이트 과정은 여기서 중지됩니다.



```
getSnapshotBeforeUpdate(prevProps, prevState) {
    if(prevState.array != = this.state.array) {
    const { scrollTop, scrollHeight } = this.list
      return { scrollTop, scrollHeight };
    }
}
```

메서드는 render에서 만들어진 결과물이 브라우저에 실제로 반영되기 직전에 호출됩니다. 이 메서드에서 반환하는 값은 componentDidUpdate에서 세 번째 파라미터인 snapshot 값으로 전달받을 수 있는데요. 주로 업데이트하기 직전의 값을 참고할 일이 있을 때 활용됩니다



```
componentDidUpdate(prevProps, prevState, snapshot) { ... }
```

 리렌더링을 완료한 후 실행합니다. 업데이트가 끝난 직후이므로, DOM 관련 처리를 해도 무방합니다. 여기서는 prevProps 또는 prevState를 사용하여 컴포넌트가 이전에 가졌던 데이터에 접근할 수 있습니다. 또 getSnapshotBeforeUpdate에서 반환한 값이 있다면 여기서 snapshot 값을 전달받을 수 있습니다.



```
componentWillUnmount() { ... }
```

컴포넌트를 DOM에서 제거할 때 실행합니다. componentDidMount에서 등록한 이벤트, 타이머, 직접 생성한 DOM이 있다면 여기서 제거 작업을 해야 합니다.



```
componentDidCatch(error, info) {
  this.setState({
      error: true
  });
  console.log({ error, info });
}
```

컴포넌트 렌더링 도중에 에러가 발생했을 때 애플리케이션이 먹통이 되지 않고 오류 UI를 보여 줄 수 있게 해 줍니다. 사용 방법은 다음과 같습니다.



### 사용예

```javascript
import React, { Component } from ‘react‘;

class LifeCycleSample extends Component {
  state = {
    number: 0,
    color: null,
  }

  myRef = null; // ref를 설정할 부분

constructor(props) {
    super(props);
    console.log(‘constructor‘);
  }

  static getDerivedStateFromProps(nextProps, prevState) {
    console.log(‘getDerivedStateFromProps’); 
      //color 값 state에 동기화
    if(nextProps.color != = prevState.color) {
      return { color: nextProps.color };
    }
    return null;
  }

componentDidMount() {
    console.log(‘componentDidMount‘);
  }

shouldComponentUpdate(nextProps, nextState) {
    console.log(‘shouldComponentUpdate‘, nextProps, nextState);
    // 숫자의 마지막 자리가 4면 리렌더링하지 않습니다.
    return nextState.number % 10 != = 4;
  }

componentWillUnmount() {
    console.log(‘componentWillUnmount‘);
  }

handleClick = () => {
    this.setState({
      number: this.state.number + 1
    });
  }

//DOM에 변화가 일어나기 전에 색상값을 snapshot값으로 변환하여 이것을 componentDidUpdate에서 조회 가능하게 함
getSnapshotBeforeUpdate(prevProps, prevState) {
    console.log(‘getSnapshotBeforeUpdate‘);
    if(prevProps.color != = this.props.color) {
      return this.myRef.style.color;
    }
    return null;
  }

componentDidUpdate(prevProps, prevState, snapshot) {
    console.log(‘componentDidUpdate‘, prevProps, prevState);
    if(snapshot) {
      console.log(‘업데이트되기 직전 색상: ‘, snapshot);
    }
  }

render() {
    console.log(‘render‘);

    const style = {
      color: this.props.color
    };

    return (
      <div>
        <h1 style={style} ref={ref => this.myRef=ref}>
          {this.state.number}
        </h1>
        <p>color: {this.state.color}</p>
        <button onClick={this.handleClick}>
          더하기
        </button>
      </div>
    )
  }
}

export default LifeCycleSample;
```



```javascript
import React, { Component } from 'react';
 
class ErrorBoundary extends Component {
  state = {
    error: false
  };
  componentDidCatch(error, info) {
    this.setState({
      error: true
    });
    console.log({ error, info });
  }
  render() {
    if (this.state.error) return <div>에러가 발생했습니다!</div>;
    return this.props.children;
  }
}
 
export default ErrorBoundary;


import React, { Component } from ‘react‘;
import LifeCycleSample from ‘./LifeCycleSample‘;

// 랜덤 색상을 생성합니다.
function getRandomColor() {
  return ‘#‘ + Math.floor(Math.random() * 16777215).toString(16);
}

class App extends Component {
  state = {
    color: ‘#000000‘
  }

handleClick = () => {
    this.setState({
      color: getRandomColor()
    });
  }

render() {
    return (
      <div>
        <button onClick={this.handleClick}>랜덤 색상</button>
        <ErrorBoundary>
          <LifeCycleSample color={this.state.color} />
        </ErrorBoundary>
      </div>
    );
  }
}

export default App;
```



에러가 발생하면 componentDidCatch 메서드가 호출되며, 이 메서드는 this.state.error 값을 true로 업데이트해 줍니다. 그리고 render 함수는 this.state.error 값이 true라면 에러가 발생했음을 알려 주는 문구를 보여 줍니다.





### Hooks

 함수형 컴포넌트에서도 상태 관리를 할 수 있는 useState, 렌더링 직후 작업을 설정하는 useEffect 등의 기능을 제공하여 기존의 함수형 컴포넌트에서 할 수 없었던 다양한 작업을 할 수 있게 해 줍니다.



하나의 **useState** 함수는 하나의 상태 값만 관리할 수 있습니다. 컴포넌트에서 관리해야 할 상태가 여러 개라면 useState를 여러 번 사용하면 됩니다.



**useEffect**는 리액트 컴포넌트가 렌더링될 때마다 특정 작업을 수행하도록 설정할 수 있는 Hook입니다. 클래스형 컴포넌트의 componentDidMount와 componentDidUpdate를 합친 형태로 보아도 무방합니다.



```javascript
import React, { useState } from ‘react‘;


const Info = () => {
  const [name, setName] = useState(“);
  const [nickname, setNickname] = useState(“);
   useEffect(() => {
    console.log(‘렌더링이 완료되었습니다!’);
    console.log({
      name,
      nickname
    });
       
    useEffect(() => {
    console.log('마운트될 때만 실행됩니다.');
 	 }, []);
       
    //name 값이 변경될 때만
    useEffect(() => {
    console.log(name);
  	}, [name]);
       
  });
    
    //뒷정리
  useEffect(() => {
    console.log(‘effect‘);
    console.log(name);
    return () => {
      console.log(‘cleanup‘);
      console.log(name);
    };
  });
    
  const onChangeName = e => {
    setName(e.target.value);
  };
 
  const onChangeNickname = e => {
    setNickname(e.target.value);
  };
 
  return (
    <div>
      <div>
        <input value={name} onChange={onChangeName} />
        <input value={nickname} onChange={onChangeNickname} />
      </div>
      <div>
        <div>
          <b>이름:</b> {name}
        </div>
        <div>
          <b>닉네임:</b> {nickname}
        </div>
      </div>
    </div>
  );
};


import React, { useState } from 'react';
import Info from './Info';
 
const App = () => {
const [visible, setVisible] = useState(false);
return (
  <div>
      <button
      onClick={() => {
        setVisible(!visible);
        }}
      >
        {visible ? '숨기기' : '보이기'}
      </button>
      <hr />
    {visible && <Info />}
    </div>
);
};
 
export default App;
```



useReducer는 useState보다 더 다양한 컴포넌트 상황에 따라 다양한 상태를 다른 값으로 업데이트해 주고 싶을 때 사용하는 Hook입니다



```javascript
import React, { useReducer } from ‘react‘;

function reducer(state, action) {
  // action.type에 따라 다른 작업 수행
  switch (action.type) {
    case ‘INCREMENT‘:
      return { value: state.value + 1 };
    case ‘DECREMENT‘:
      return { value: state.value - 1 };
    default:
      // 아무것도 해당되지 않을 때 기존 상태 반환
      return state;
  }
}

const Counter = () => {
  const [state, dispatch] = useReducer(reducer, { value: 0 });

return (
    <div>
      <p>
        현재 카운터 값은 <b>{state.value}</b>입니다.
      </p>
      <button onClick={() => dispatch({ type: ‘INCREMENT‘ })}>+1</button>
      <button onClick={() => dispatch({ type: ‘DECREMENT‘ })}>-1</button>
    </div>
  );
};

export default Counter;
```

useReducer의 첫 번째 파라미터에는 리듀서 함수를 넣고, 두 번째 파라미터에는 해당 리듀서의 기본값을 넣어 줍니다. 이 Hook을 사용하면 state 값과 dispatch 함수를 받아 오는데요. 여기서 state는 현재 가리키고 있는 상태고, dispatch는 액션을 발생시키는 함수입니다. dispatch(action)과 같은 형태로, 함수 안에 파라미터로 액션 값을 넣어 주면 리듀서 함수가 호출되는 구조입니다.

useReducer를 사용했을 때의 가장 큰 장점은 컴포넌트 업데이트 로직을 컴포넌트 바깥으로 빼낼 수 있다는 것입니다.





useMemo를 사용하면 함수형 컴포넌트 내부에서 발생하는 연산을 최적화할 수 있습니다. 



```javascript
import React, { useState } from ‘react‘;

const getAverage = numbers => {
  console.log(‘평균값 계산 중..‘);
  if (numbers.length === 0) return 0;
  const sum = numbers.reduce((a, b) => a + b);
  return sum / numbers.length;
};

const Average = () => {
  const [list, setList] = useState([]);
  const [number, setNumber] = useState(“);
 
  const onChange = e => {
    setNumber(e.target.value);
  };
  const onInsert = e => {
    const nextList = list.concat(parseInt(number));
    setList(nextList);
    setNumber(“);
  };
    
    const onChange = useCallback(e => {
    setNumber(e.target.value);
}, []); // 컴포넌트가 처음 렌더링될 때만 함수 생성
const onInsert = useCallback(() => {
    const nextList = list.concat(parseInt(number));
    setList(nextList);
    setNumber(“);
  }, [number, list]); // number 혹은 list가 바뀌었을 때만 함수 생성
    
    
 
    //랜더링 하는 과정에서 특정 값이 변할때만 연산을 실행
    const avg = useMemo(() => getAverage(list), [list]);
    
  return (
    <div>
      <input value={number} onChange={onChange} />
      <button onClick={onInsert}>등록</button>
      <ul>
        {list.map((value, index) => (
          <li key={index}>{value}</li>
        ))}
      </ul>
      <div>
        <b>평균값:</b> {avg}
      </div>
    </div>
  );
};

export default Average;
```





useCallback은 useMemo와 상당히 비슷한 함수입니다. 주로 렌더링 성능을 최적화해야 하는 상황에서 사용하는데요. 이 Hook을 사용하면 이벤트 핸들러 함수를 필요할 때만 생성할 수 있습니다.



 onChange와 onInsert라는 함수를 선언해 주었지요? 이렇게 선언하면 컴포넌트가 리렌더링될 때마다 이 함수들이 새로 생성됩니다. 대부분의 경우 이러한 방식은 문제없지만, 컴포넌트의 렌더링이 자주 발생하거나 렌더링해야 할 컴포넌트의 개수가 많아지면 이 부분을 최적화해 주는 것이 좋습니다.





useCallback은 결국 useMemo로 함수를 반환하는 상황에서 더 편하게 사용할 수 있는 Hook입니다. 숫자, 문자열, 객체처럼 일반 값을 재사용하려면 useMemo를 사용하고, 함수를 재사용하려면 useCallback을 사용하세요.

```javascript
useCallback(() => {
  console.log(‘hello world!‘);
}, [])

useMemo(() => {
  const fn = () => {
    console.log(‘hello world!‘);
  };
  return fn;
}, [])
```



useRef Hook은 함수형 컴포넌트에서 ref를 쉽게 사용할 수 있도록 해 줍니다. Average 컴포넌트에서 **등록** 버튼을 눌렀을 때 포커스가 인풋 쪽으로 넘어가도록 코드를 작성해 보겠습니다.



```javascript
import React, { useState, useMemo, useCallback, useRef } from ‘react‘;

const getAverage = numbers => {
  console.log(‘평균값 계산 중..‘);
  if (numbers.length === 0) return 0;
  const sum = numbers.reduce((a, b) => a + b);
  return sum / numbers.length;
};

const Average = () => {
  const [list, setList] = useState([]);
  const [number, setNumber] = useState(“);
  const inputEl = useRef(null);

const onChange = useCallback(e => {
    setNumber(e.target.value);
  }, []); // 컴포넌트가 처음 렌더링될 때만 함수 생성
const onInsert = useCallback(() => {
    const nextList = list.concat(parseInt(number));
    setList(nextList);
    setNumber(“);
    inputEl.current.focus();
  }, [number, list]); // number 혹은 list가 바뀌었을 때만 함수 생성

const avg = useMemo(() => getAverage(list), [list]);

return (
    <div>
      <input value={number} onChange={onChange} ref={inputEl} />
      <button onClick={onInsert}>등록</button>
      <ul>
        {list.map((value, index) => (
          <li key={index}>{value}</li>
        ))}
      </ul>
      <div>
        <b>평균값:</b> {avg}
      </div>
    </div>
  );
};

export default Average;
```





### 컴포넌트 스타일링





### To Do App

**1.** **TodoTemplate**: 화면을 가운데에 정렬시켜 주며, 앱 타이틀(일정 관리)을 보여 줍니다. children으로 내부 JSX를 props로 받아 와서 렌더링해 줍니다.

**2.** **TodoInsert**: 새로운 항목을 입력하고 추가할 수 있는 컴포넌트입니다. state를 통해 인풋의 상태를 관리합니다.

**3.** **TodoListItem**: 각 할 일 항목에 대한 정보를 보여 주는 컴포넌트입니다. todo 객체를 props로 받아 와서 상태에 따라 다른 스타일의 UI를 보여 줍니다.

**4.** **TodoList**: todos 배열을 props로 받아 온 후, 이를 배열 내장 함수 map을 사용해서 여러 개의 TodoListItem 컴포넌트로 변환하여 보여 줍니다.





### 컴포넌트 성능 최적화





### API 호출

