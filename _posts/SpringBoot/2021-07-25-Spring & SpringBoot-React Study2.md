### State



리액트에서 state는 컴포넌트 내부에서 바뀔 수 있는 값을 의미합니다. props는 컴포넌트가 사용되는 과정에서 부모 컴포넌트가 설정하는 값이며, 컴포넌트 자신은 해당 props를 읽기 전용으로만 사용할 수 있습니다. props를 바꾸려면 부모 컴포넌트에서 바꾸어 주어야 합니다. 예를 들어 현재 상황에서는 App 컴포넌트에서 MyComponent를 사용할 때 props를 바꾸어 주어야 값이 변경될 수 있는 것이죠. 반면 MyComponent에서는 전달받은 name 값을 직접 바꿀 수 없습니다.

```javascript
import React, { Component } from ‘react‘;
 
class Counter extends Component {
  constructor(props) { //stat 설정할때는 constructor 메서드 사용
    super(props); //현재 클래스형 컴포넌트가 상속하고 있는 리액트의 컴포넌트 생성자 호출
    // state의 초깃값 설정하기
    this.state = { //객체 형식
      number: 0
      fixedNumber: 0
    };
  }
    //다른 방법
  state = {
    number: 0,
    fixedNumber: 0
  };
  render() {
    const { number } = this.state; // state를 조회할 때는 this.state로 조회합니다.
    return (
      <div>
        <h1>{number}</h1>
         <h2>바뀌지 않는 값: {fixedNumber}</h2>
        <button
          // onClick을 통해 버튼이 클릭되었을 때 호출할 함수를 지정합니다.
          onClick={() => {
            // this.setState를 사용하여 state에 새로운 값을 넣을 수 있습니다.
            this.setState({ number: number + 1 });
            this.setState({ number: this.state.number + 1 }); //이렇게 작성하면 비동기로 동작하기 때문에 1씩 더해진다
              //함수로 작성
              this.setState(prevState => ({
      			number: prevState.number + 1
			    }));
              
              //setState이후에 callback 동작 지정
              this.setState(
                  {
                    number: number + 1
                  },
                  () => {
                    console.log(‘방금 setState가 호출되었습니다.’);
                    console.log(this.state);
                  }
                );
              
          }}
        >
          +1
        </button>
      </div>
    );
  }
}
 
export default Counter;

import React from ‘react‘;
import Counter from ‘./Counter‘;

const App = () => {
  return <Counter />;
};

export default App;
```



배열 비구조화 활당

```
const array = [1, 2];
const one = array[0];
const two = array[1];

const [one, two] = array;
```

props는 부모 컴포넌트가 설정하고, state는 컴포넌트 자체적으로 지닌 값으로 컴포넌트 내부에서 값을 업데이트할 수 있습니다.

props를 사용한다고 해서 값이 무조건 고정적이지는 않습니다. 부모 컴포넌트의 state를 자식 컴포넌트의 props로 전달하고, 자식 컴포넌트에서 특정 이벤트가 발생할 때 부모 컴포넌트의 메서드를 호출하면 props도 유동적으로 사용할 수 있습니다

앞으로 새로운 컴포넌트를 만들 때는 useState를 사용할 것을 권장합니다. 이로써 코드가 더 간결해질 뿐만 아니라, 리액트 개발 팀이 함수형 컴포넌트와 Hooks를 사용하는 것이 주요 컴포넌트 개발 방식이 될 것이라고 공지했기 때문입니다.



### 이벤트 핸들링



```javascript
import React, { useState } from ‘react‘;


const Say = () => {
  const [message, setMessage] = useState(“);
  const onClickEnter = () => setMessage(‘안녕하세요!’);
  const onClickLeave = () => setMessage(‘안녕히 가세요!’);



const [color, setColor] = useState(‘black‘);



return (
    <div>
      <button onClick={onClickEnter}>입장</button>
      <button onClick={onClickLeave}>퇴장</button>
      <h1 style={{ color }}>{message}</h1>
      <button style={{ color: ‘red‘ }} onClick={() => setColor(‘red‘)}>
        빨간색
      </button>
      <button style={{ color: ‘green‘ }} onClick={() => setColor(‘green‘)}>
        초록색
      </button>
      <button style={{ color: ‘blue‘ }} onClick={() => setColor(‘blue‘)}>
        파란색
      </button>
    </div>
  );
};



export default Say;
```



**1.** **이벤트 이름은 카멜 표기법으로 작성합니다**

**2.** **이벤트에 실행할 자바스크립트 코드를 전달하는 것이 아니라, 함수 형태의 값을 전달합니다**

**3.** **DOM 요소에만 이벤트를 설정할 수 있습니다**

MyComponent에 onClick 값을 설정한다면 MyComponent를 클릭할 때 doSomething 함수를 실행하는 것이 아니라, 그냥 이름이 onClick인 props를 MyComponent에게 전달해 줄 뿐입니다.



이벤트 종류

• Clipboard

• Composition

• Keyboard

• Focus

• Form

• Mouse

• Selection

• Touch

• UI

• Wheel

• Media

• Image

• Animation

• Transition



### 이벤트 핸들링

```javascript
import React, { Component } from ‘react‘;


class EventPractice extends Component {
    
  state = {
    message: ''
  }

  render() {
    return (
      <div>
        <h1>이벤트 연습</h1>
		<input
          type="text"
          name="message"
          placeholder="아무거나 입력해 보세요"
          value={this.state.message}
          onChange={
            (e) => { //e 객체는 SyntheticEvent로 웹 브라우저의 네이티브 이벤트를 감싸는 객체
              this.setState({
                message: e.target.value
              })
            }
          }
        />
		<button onClick={
          () => {
            alert(this.state.message);
            this.setState({
              message: “
            });
          }
        }>확인</button>
      </div>
    );
  }
}



export default EventPractice;

import React from 'react';
import EventPractice from './EventPractice';
 
const App = () => {
return <EventPractice />;
};
 
export default App;
```

 이 방법 대신 함수를 미리 준비하여 전달하는 방법도 있습니다. 성능상으로는 차이가 거의 없지만, 가독성은 훨씬 높습니다



```javascript
import React, { Component } from ‘react‘;

class EventPractice extends Component {

state = {
    message: “
  }

constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.handleClick = this.handleClick.bind(this);
  }

handleChange(e) {
    this.setState({
      message: e.target.value
    });
  }

handleClick() {
    alert(this.state.message);
    this.setState({
      message: “
    });
  }

render() {
    return (
      <div>
        <h1>이벤트 연습</h1>
        <input
          type=“text“
          name=“message“
          placeholder=“아무거나 입력해 보세요“
          value={this.state.message}
          onChange={this.handleChange}
        />
        <button onClick={this.handleClick}>확인</button>
      </div>
    );
  }
}


export default EventPractice;
```

함수가 호출될 때 this는 호출부에 따라 결정되므로, 클래스의 임의 메서드가 특정 HTML 요소의 이벤트로 등록되는 과정에서 메서드와 this의 관계가 끊어져 버립니다. 이 때문에 임의 메서드가 이벤트로 등록되어도 this를 컴포넌트 자신으로 제대로 가리키기 위해서는 메서드를 this와 바인딩(binding)하는 작업이 필요합니다. 만약 바인딩하지 않는 경우라면 this가 undefined를 가리키게 됩니다.



Property Initializer Syntax

바벨의 transform-class-properties 문법을 사용하여 화살표 함수 형태로 메서드를 정의하는 것입니다.

```javascript
import React, { Component } from ‘react‘;

class EventPractice extends Component {

state = {
    username: “,
    message: “
  }

handleChange = (e) => {
    this.setState({
      [e.target.name]: e.target.value //e.target.name으로 input 의 이름을 가져올수 있다
    });
  }

handleClick = () => {
    alert(this.state.username + ‘: ‘ + this.state.message);
    this.setState({
      username: “,
      message: “
    });
  }

handleKeyPress = (e) => {
    if(e.key === 'Enter') {
        this.handleClick();
    }
}

render() {
    return (
      <div>
        <h1>이벤트 연습</h1>
        <input 
          type=“text“
          name=“username“
          placeholder=“사용자명“
          value={this.state.username}
          onChange={this.handleChange}
        />
        <input 
          type=“text“
          name=“message“
          placeholder=“아무거나 입력해 보세요“
          value={this.state.message}
          onChange={this.handleChange}
          onKeyPress={this.handleKeyPress}
        />
        <button onClick={this.handleClick}>확인</button>
      </div>
    );
  }
}



export default EventPractice;
```





함수형으로 구현

```javascript
import React, { useState } from ‘react‘;

const EventPractice = () => {
  const [username, setUsername] = useState(“);
  const [message, setMessage] = useState(“);
    
  const onChangeUsername = e => setUsername(e.target.value);
  const onChangeMessage = e => setMessage(e.target.value);
    
  const onClick = () => {
    alert(username + ‘: ‘ + message);
    setUsername(“);
    setMessage(“);
  };
    
  const onKeyPress = e => {
    if (e.key === ‘Enter‘) {
      onClick();
    }
  };
    
  return (
    <div>
      <h1>이벤트 연습</h1>
      <input
        type=“text”
        name=“username“
        placeholder=“사용자명“
        value={username}
        onChange={onChangeUsername}
      />
      <input
        type=“text“
        name=“message“
        placeholder=“아무거나 입력해 보세요“
        value={message}
        onChange={onChangeMessage}
        onKeyPress={onKeyPress}
      />
      <button onClick={onClick}>확인</button>
    </div>
  );
};
export default EventPractice;
```



**useState**

```javascript
const numberState = useState(0);
const number = numberState[0];
const setNumber = numberState[1];

const [number, setNumber] = useState(0);
```



### ref: DOM에 이름 달기

DOM 요소에 어떤 작업을 해야 할 때 이렇게 요소에 id를 달면 CSS에서 특정 id에 특정 스타일을 적용하거나 자바스크립트에서 해당 id를 가진 요소를 찾아서 작업할 수 있겠죠



리액트 컴포넌트 안에서도 id를 사용할 수는 있습니다. JSX 안에서 DOM에 id를 달면 해당 DOM을 렌더링할 때 그대로 전달됩니다. 하지만 특수한 경우가 아니면 사용을 권장하지 않습니다. 예를 들어 같은 컴포넌트를 여러 번 사용한다고 가정해 보세요. HTML에서 DOM의 id는 유일(unique)해야 하는데, 이런 상황에서는 중복 id를 가진 DOM이 여러 개 생기니 잘못된 사용입니다.

ref는 전역적으로 작동하지 않고 컴포넌트 내부에서만 작동하기 때문에 이런 문제가 생기지 않습니다.

대부분은 id를 사용하지 않고도 원하는 기능을 구현할 수 있지만, 다른 라이브러리나 프레임워크와 함께 id를 사용해야 하는 상황이 발생할 수 있습니다. 이런 상황에서는 컴포넌트를 만들 때마다 id 뒷부분에 추가 텍스트를 붙여서(예: button01 button02 button03…) 중복 id가 발생하는 것을 방지해야 합니다.



 DOM에 접근하지 않아도 state로 구현할 수 있습니다.

```javascript
.success {
  background-color: lightgreen;
}
.failure {
  background-color: lightcoral;
}

import React, { Component } from ‘react‘;
import ‘./ValidationSample.css‘;

class ValidationSample extends Component {
  state = {
    password: “,
    clicked: false,
    validated: false
  }

handleChange = (e) => {
    this.setState({
      password: e.target.value
    });
  }

handleButtonClick => () => {
    this.setState({
      clicked: true,
      validated: this.state.password === ‘0000‘
    })
  }

handleButtonClick = () => {
  this.setState({
    clicked: true,
    validated: this.state.password === '0000'
  });
  this.input.focus(); //ref 에 포커스 주기
}

render() {
    return (
      <div>
        <input
          ref={(ref) => this.input=ref}
          type=“password“
          value={this.state.password}
          onChange={this.handleChange}
          className={this.state.clicked ? (this.state.validated ? ‘success’ : ‘failure’) : “}
        />
        <button onClick={this.handleButtonClick}>검증하기</button>
      </div>
    );
  }
}



export default ValidationSample;
```



컴포넌트 내부에도 ref를 달수 있다

```javascript
import React, { Component } from ‘react‘;


class ScrollBox extends Component {
  scrollToBottom = () => {
    const { scrollHeight, clientHeight } = this.box;
    /* 앞 코드에는 비구조화 할당 문법을 사용했습니다.
       다음 코드와 같은 의미입니다.
       const scrollHeight = this.box.scrollHeight;
       const clientHeight = this.box.cliengHeight;
    */
    this.box.scrollTop = scrollHeight - clientHeight;
  }
      
  render() {
    const style = {
      border: ‘1px solid black‘,
      height: ‘300px‘,
      width: ‘300px‘,
      overflow: ‘auto‘,
      position: ‘relative‘
    };

<span class="co50">const</span><span class="cd2"> innerStyle </span><span class="co40">=</span><span class="cd2"> {</span>
      width: ‘100%‘,
      height: ‘650px‘,
      background: ‘linear-gradient(white, black)‘
    }
<span class="co50">return</span><span class="cd2"> (</span>

      <div 
        style={style}
        ref={(ref) => {this.box=ref}}> //내부 DOM에 ref
        <div style={innerStyle}/>
      </div>
    );
  }
}

export default ScrollBox;

import React, { Component } from ‘react‘;
import ScrollBox from ‘./ScrollBox‘;

class App extends Component {
  render() {
    return (
      <div>
        <ScrollBox ref={(ref) => this.scrollBox=ref}/>
        <button onClick={() => this.scrollBox.scrollToBottom()}>
          맨 밑으로
        </button>
      </div>
    );
  }
}

export default App;
```

이 방법은 리액트 사상에 어긋난 설계입니다. 앱 규모가 커지면 마치 스파게티처럼 구조가 꼬여 버려서 유지 보수가 불가능하지요. 컴포넌트끼리 데이터를 교류할 때는 언제나 데이터를 부모 ↔ 자식 흐름으로 교류해야 합니다. 나중에 리덕스 혹은 Context API를 사용하여 효율적으로 교류하는 방법을 배울 것입니다.



### 컴포넌트 반복

```
arr.map(callback, [thisArg])
```

이 함수의 파라미터는 다음과 같습니다.

• callback: 새로운 배열의 요소를 생성하는 함수로 파라미터는 다음 세 가지입니다.

\- currentValue: 현재 처리하고 있는 요소

\- index: 현재 처리하고 있는 요소의 index 값

\- array: 현재 처리하고 있는 원본 배열

• thisArg(선택 항목): callback 함수 내부에서 사용할 this 레퍼런스



IterationSample

```javascript
import React from 'react';
 
const IterationSample = () => {
  const names = ['눈사람', '얼음', '눈', '바람'];
  const nameList = names.map(name => <li>{name}</li>);
  return <ul>{nameList}</ul>;
};
 
export default IterationSample;
```



### Key

key는 컴포넌트 배열을 렌더링했을 때 어떤 원소에 변동이 있었는지 알아내려고 사용합니다. 예를 들어 유동적인 데이터를 다룰 때는 원소를 새로 생성할 수도, 제거할 수도, 수정할 수도 있죠. key가 없을 때는 Virtual DOM을 비교하는 과정에서 리스트를 순차적으로 비교하면서 변화를 감지합니다. 하지만 key가 있다면 이 값을 사용하여 어떤 변화가 일어났는지 더욱 빠르게 알아낼 수 있습니다.

```javascript
import React from 'react';
 
const IterationSample = () => {
  const names = ['눈사람', '얼음', '눈', '바람'];
  const namesList = names.map((name, index) => <li key={index}>{name}</li>);
  return <ul>{namesList}</ul>;
};
 
export default IterationSample;
```



### 응용

```javascript
import React, { useState } from ‘react‘;


const IterationSample = () => {
  const [names, setNames] = useState([
    { id: 1, text: ‘눈사람‘ },
    { id: 2, text: ‘얼음‘ },
    { id: 3, text: ‘눈‘ },
    { id: 4, text: ‘바람‘ }
  ]);
  const [inputText, setInputText] = useState(“);
  const [nextId, setNextId] = useState(5); // 새로운 항목을 추가할 때 사용할 id
 
  const onChange = e => setInputText(e.target.value);
  const onClick = () => {
    const nextNames = names.concat({
      id: nextId, // nextId 값을 id로 설정하고
      text: inputText
    });
    setNextId(nextId + 1); // nextId 값에 1을 더해 준다.
    setNames(nextNames); // names 값을 업데이트한다.
    setInputText(“); // inputText를 비운다.
  };

const namesList = names.map(name => <li key={name.id}>{name.text}</li>);
  return (
    <>
      <input value={inputText} onChange={onChange} />
      <button onClick={onClick}>추가</button>
      <ul>{namesList}</ul>
    </>
  );
};

export default IterationSample;
```

https://thebook.io/080203/ch06/04/03/