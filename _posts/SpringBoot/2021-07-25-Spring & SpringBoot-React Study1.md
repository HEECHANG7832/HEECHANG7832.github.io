
### 리액트란

구조가 MVC, MVW 등인 프레임워크와 달리, **오직 V**(View)**만 신경 쓰는 라이브러리입니다.**

리액트 프로젝트에서 특정 부분이 어떻게 생길지 정하는 선언체가 있는데, 이를 컴포넌트(component)라고 합니다.

컴포넌트는 재사용이 가능한 API로 수많은 기능들을 내장하고 있으며, 컴포넌트 하나에서 해당 컴포넌트의 생김새와 작동 방식을 정의합니다.

사용자 화면에 뷰를 보여 주는 것을 렌더링이라고 합니다.



```
render() { ... }
```

이 함수는 컴포넌트가 어떻게 생겼는지 정의하는 역할을 합니다. 이 함수는 html 형식의 문자열을 반환하지 않고, 뷰가 어떻게 생겼고 어떻게 작동하는지에 대한 정보를 지닌 객체를 반환합니다. 컴포넌트 내부에는 또 다른 컴포넌트들이 들어갈 수 있습니다. 이때 render 함수를 실행하면 그 내부에 있는 컴포넌트들도 재귀적으로 렌더링합니다. 이렇게 최상위 컴포넌트의 렌더링 작업이 끝나면 지니고 있는 정보들을 사용하여 HTML 마크업(markup)을 만들고, 이를 우리가 정하는 실제 페이지의 DOM 요소 안에 주입합니다.



 “조화 과정(reconciliation)을 거친다”라고 하는 것이 더 정확한 표현입니다. 컴포넌트에서 데이터에 변화가 있을 때 우리가 보기에는 변화에 따라 뷰가 변형되는 것처럼 보이지만, 사실은 새로운 요소로 갈아 끼우기 때문입니다.



자바스크립트를 사용하여 두 가지 뷰를 최소한의 연산으로 비교한 후, 둘의 차이를 알아내 최소한의 연산으로 DOM 트리를 업데이트하는 것이죠.



### Virtual DOM

 DOM은 Document Object Model의 약어입니다. 즉, 객체로 문서 구조를 표현하는 방법으로 XML이나 HTML로 작성합니다.

 HTML은 자체적으로는 정적입니다. 자바스크립트를 사용하여 이를 동적으로 만들 수 있지요.

모가 큰 웹 애플리케이션에서 DOM에 직접 접근하여 변화를 주다 보면 성능 이슈가 조금씩 발생하기 시작합니다

DOM 자체는 빠릅니다. DOM 자체를 읽고 쓸 때의 성능은 자바스크립트 객체를 처리할 때의 성능과 비교하여 다르지 않습니다. 단, 웹 브라우저 단에서 DOM에 변화가 일어나면 웹 브라우저가 CSS를 다시 연산하고, 레이아웃을 구성하고, 페이지를 리페인트합니다. 이 과정에서 시간이 허비되는 것입니다.

HTML 마크업을 시각적인 형태로 변환하는 것은 웹 브라우저가 하는 주 역할이기 때문에, 이를 처리할 때 컴퓨터 자원을 사용하는 것은 어쩔 수 없습니다. DOM을 조작할 때마다 엔진이 웹 페이지를 새로 그리기 때문에 업데이트가 너무 잦으면 성능이 저하될 수 있습니다. 이런 문제를 해결하려면 DOM을 아예 조작하지 않을 수 없겠지요? 그 대신 DOM을 최소한으로 조작하여 작업을 처리하는 방식으로 개선할 수 있습니다.

리액트는 Virtual DOM 방식을 사용하여 DOM 업데이트를 추상화함으로써 DOM 처리 횟수를 최소화하고 효율적으로 진행합니다.

Virtual DOM을 사용하면 실제 DOM에 접근하여 조작하는 대신, 이를 추상화한 자바스크립트 객체를 구성하여 사용합니다. 마치 실제 DOM의 가벼운 사본과 비슷하죠.

리액트에서 데이터가 변하여 웹 브라우저에 실제 DOM을 업데이트할 때는 다음 세 가지 절차를 밟습니다.

**1.** 데이터를 업데이트하면 전체 UI를 Virtual DOM에 리렌더링합니다.

**2.** 이전 Virtual DOM에 있던 내용과 현재 내용을 비교합니다.

**3.** 바뀐 부분만 실제 DOM에 적용합니다.



지속적으로 데이터가 변화하는 대규모 애플리케이션 구축하기

예, 그렇습니다. 결국에는 적절한 곳에 사용해야 리액트가 지닌 진가를 비로소 발휘할 수 있습니다. 리액트를 사용하지 않아도 코드 최적화를 열심히 하면 DOM 작업이 느려지는 문제를 개선할 수 있고, 또 작업이 매우 간단할 때는(예: 단순 라우팅 정도만 있는 정적인 페이지) 오히려 리액트를 사용하지 않는 편이 더 나은 성능을 보이기도 합니다



리액트와 Virtual DOM이 언제나 제공할 수 있는 것은 바로 업데이트 처리 간결성입니다. UI를 업데이트하는 과정에서 생기는 복잡함을 모두 해소하고, 더욱 쉽게 업데이트에 접근할 수 있습니다.



리액트 애플리케이션은 웹 브라우저에서 실행되는 코드이므로 Node.js와 직접적인 연관은 없지만, 프로젝트를 개발하는 데 필요한 주요 도구들이 Node.js를 사용하기 때문에 설치하는 것입니다. 이때 사용하는 개발 도구에는 ECMAScript 6(2015년 공식적으로 업데이트한 자바스크립트 문법이며, 리액트를 공부하면서 주요 내용을 틈틈이 소개합니다)를 호환시켜 주는 바벨(babel), 모듈화된 코드를 한 파일로 합치고(번들링) 코드를 수정할 때마다 웹 브라우저를 리로딩하는 등의 여러 기능을 지닌 웹팩(webpack) 등이 있습니다. 책 후반부에서는 Node.js를 사용하여 백엔드 서버를 구현합니다.

```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
nvm –version

nvm install –lts

//mac
Homebrew를 사용하여 yarn을 설치합니다.
usr/bin/ruby -e “$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

$ brew update
$ brew install yarn –without-node
$ yarn config set prefix ~/.yarn
$ echo ‘export PATH=“$(yarn global bin):$PATH”’ >> ~/.bash_profile

//ubuntu
$ curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
$ echo “deb https://dl.yarnpkg.com/debian/ stable main” | sudo tee /etc/apt/sources. list.d/yarn.list
$ sudo apt-get update && sudo apt-get install –no-install-recommends yarn
$ echo ‘export PATH=“$(yarn global bin):$PATH”’ >> ~/.bashrc
```



VS Code 확장프로그램

**ESLint**

**Reactjs Code Snippets**

**Prettier**-**Code formatter**



리액트 프로젝트 생성

```
yarn create react-app hello-react
//또는 $ npm init react-app <프로젝트 이름>

프로젝트 시작
$ cd hello-react
$ yarn start # 또는 npm start
```



### JSX

src/app.js

```javascript
import React from ‘react‘;
import logo from ‘./logo.svg‘;
import ‘./App.css‘;


function App() {
  return (
    <div className=“App“>
      <header className=“App-header“>
        <img src={logo} className=“App-logo“ alt=“logo“ />
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
        <a
          className=“App-link“
          href=“https://reactjs.org“
          target=“_blank“
          rel=“noopener noreferrer“
        >
          Learn React
        </a>
      </header>
    </div>
  );
}



export default App;
```

이 코드는 리액트를 불러와서 사용할 수 있게 해 줍니다. 리액트 프로젝트를 만들 때 node_modules라는 디렉터리도 함께 생성되는데요. 프로젝트 생성 과정에서 node_modules 디렉터리에 react 모듈이 설치됩니다. 그리고 이렇게 import 구문을 통해 리액트를 불러와서 사용할 수 있는 것이죠.

Node.js에서 지원하는 기능입니다.



대표적인 번들러로 웹팩, Parcel, browserify라는 도구들이 있으며, 각 도구마다 특성이 다릅니다. 리액트 프로젝트에서는 주로 웹팩을 사용하는 추세입니다. 편의성과 확장성이 다른 도구보다 뛰어나기 때문입니다. 번들러 도구를 사용하면 import(또는 require)로 모듈을 불러왔을 때 불러온 모듈을 모두 합쳐서 하나의 파일을 생성해 줍니다. 또 최적화 과정에서 여러 개의 파일로 분리될 수도 있습니다.



함수에서 반환하는 내용을 보면 마치 HTML을 작성한 것 같지요? 하지만 이 코드는 HTML이 아닙니다. 그렇다고 문자열 템플릿도 아닙니다. 이런 코드는 JSX라고 부릅니다.

JSX는 자바스크립트의 확장 문법이며 XML과 매우 비슷하게 생겼습니다. 이런 형식으로 작성한 코드는 브라우저에서 실행되기 전에 코드가 번들링되는 과정에서 바벨을 사용하여 일반 자바스크립트 형태의 코드로 변환됩니다.



ReactDOM.render는 무엇인가요?

이 코드는 컴포넌트를 페이지에 렌더링하는 역할을 하며, react-dom 모듈을 불러와 사용할 수 있습니다. 이 함수의 첫 번째 파라미터에는 페이지에 렌더링할 내용을 JSX 형태로 작성하고, 두 번째 파라미터에는 해당 JSX를 렌더링할 document 내부 요소를 설정합니다. 여기서는 id가 root인 요소 안에 렌더링을 하도록 설정했는데요. 이 요소는 public/index.html 파일을 열어 보면 있답니다.



반드시 부모 요소 하나로 감싸야 합니다.

 꼭 div 요소를 사용하고 싶지 않을 수도 있습니다. 그런 경우에는 리액트 v16 이상 부터 도입된 Fragment라는 기능을 사용하면 됩니다.



JSX가 단순히 DOM 요소를 렌더링하는 기능밖에 없었다면 뭔가 좀 아쉬웠을 것입니다. JSX 안에서는 자바스크립트 표현식을 쓸 수 있습니다.

```javascript
import React from 'react';

function App() {
  const name = '리액트';
  return (
    <>
      <h1>{name} 안녕!</h1>
      <h2>잘 작동하니?</h2>
    </>

      //조건에 따른 랜더링
       <div>
      {name === ‘리액트‘ ? (
        <h1>리액트입니다.</h1>
      ) : (
        <h2>리액트가 아닙니다.</h2>
      )}
    </div>

//AND를 사용한 조건부 랜더링
  const name = ‘뤼왝트‘;
  return <div>{name === ‘리액트‘ && <h1>리액트입니다.</h1>}</div>;
  );
}

export default App;
```

const는 ES6 문법에서 새로 도입되었으며 한번 지정하고 나면 변경이 불가능한 상수를 선언할 때 사용하는 키워드입니다. let은 동적인 값을 담을 수 있는 변수를 선언할 때 사용하는 키워드입니다.

let과 const는 scope가 함수 단위가 아닌 블록 단위이므로, if 문 내부에서 선언한 a 값은 if 문 밖의 a 값을 변경하지 않습니다.

let과 const를 사용할 때 같은 블록 내부에서 중복 선언이 불가능하다는 점에 주의하세요.

일단 ES6 문법에서 var을 사용할 일은 없습니다.

편하게 생각하면 기본적으로 const를 사용하고, 해당 값을 바꾸어야 할 때는 let을 사용하면 되겠습니다.



리액트에서 DOM 요소에 스타일을 적용할 때는 문자열 형태로 넣는 것이 아니라 객체 형태로 넣어 주어야 합니다. 스타일 이름 중에서 background-color처럼 - 문자가 포함되는 이름이 있는데요. 이러한 이름은 - 문자를 없애고 카멜 표기법(camelCase)으로 작성해야 합니다. 따라서 background-color는 backgroundColor로 작성합니다.

```javascript
import React from ‘react‘;


function App() {
  const name = ‘리액트‘;
  const style = {
    // background-color는 backgroundColor와 같이 -가 사라지고 카멜 표기법으로 작성됩니다.
    backgroundColor: ‘black‘,
    color: ‘aqua‘,
    fontSize: ‘48px‘, // font-size -> fontSize
    fontWeight: ‘bold‘, // font-weight -> fontWeight
    padding: 16 // 단위를 생략하면 px로 지정됩니다.
  };
  return <div style={style}>{name} </div>;

 const name = '리액트';
  return (
    <div
      style={{
        // background-color는 backgroundColor와 같이 -가 사라지고 카멜 표기법으로 작성됩니다.
        backgroundColor: 'black',
        color: 'aqua',
        fontSize: '48px', // font-size -> fontSize
        fontWeight: 'bold', // font-weight -> fontWeight
        padding: 16 // 단위를 생략하면 px로 지정됩니다.
      }}
    >
      {name}
    </div>
  );

//css class 설정시 className으로 해줘야함
     const name = '리액트';
	  return <div className="react">{name}</div>;

{/* … */}
}



export default App;
```



### 컴포넌트

리액트를 사용하여 애플리케이션의 인터페이스를 설계할 때 사용자가 볼 수 있는 요소는 여러 가지 컴포넌트로 구성되어 있습니다. 예를 들어 뒤에서 만들어 볼 일정 관리 애플리케이션을 미리 살펴봅시다.



TodoTemplate 컴포넌트입니다. 이 컴포넌트는 현재 화면의 중앙에 있는 사각형 레이아웃의 역할을 하고 있습니다. 그리고 새로운 항목을 추가할 수 있는 TodoInput 컴포넌트입니다. 위 화면에서는 검정색 영역이 바로 TodoInput입니다. 그리고 할 일 항목을 여러 개 보여 주는 TodoList 컴포넌트입니다. 마지막으로 TodoList에서 각 항목을 보여 주기 위해 사용되는 TodoItem 컴포넌트입니다.

컴포넌트의 기능은 단순한 템플릿 이상입니다. 데이터가 주어졌을 때 이에 맞추어 UI를 만들어 주는 것은 물론이고, 라이프사이클 API를 이용하여 컴포넌트가 화면에서 나타날 때, 사라질 때, 변화가 일어날 때 주어진 작업들을 처리할 수 있으며, 임의 메서드를 만들어 특별한 기능을 붙여 줄 수 있습니다.



```javascript
//함수형 컴포넌트
import React from ‘react‘;
import ‘./App.css‘;


function App() {
  const name = ‘리액트‘;
  return <div className=“react“>{name}</div>;
}

export default App;


//클래스형 컴포넌트
import React, { Component } from 'react';

class App extends Component {
  render() {
    const name = 'react';
    return <div className="react">{name}</div>;
  }
}

export default App;
```



ES6에 추가된 기능

```javascript
function Dog(name) {
  this.name = name;
}

Dog.prototype.say = function() {
  console.log(this.name + ': 멍멍');
}
var dog = new Dog('검둥이');
dog.say(); // 검둥이: 멍멍


ES6 문법부터는 이것과 기능이 똑같은 코드를 class를 사용하여 다음과 같이 작성할 수 있습니다.

class Dog {
  constructor(name) {
     this.name = name;
  }
  say() {
     console.log(this.name + ': 멍멍');
  }
}

const dog = new Dog('흰둥이');
dog.say(); // 흰둥이: 멍멍
```





클래스형 컴포넌트에서는 render 함수가 꼭 있어야 하고, 그 안에서 보여 주어야 할 JSX를 반환해야 합니다.

함수형 컴포넌트의 장점을 나열해 보면 다음과 같습니다. 우선 클래스형 컴포넌트보다 선언하기가 훨씬 편합니다. 메모리 자원도 클래스형 컴포넌트보다 덜 사용합니다. 또한, 프로젝트를 완성하여 빌드한 후 배포할 때도 함수형 컴포넌트를 사용하는 것이 결과물의 파일 크기가 더 작습니다(함수형 컴포넌트와 클래스형 컴포넌트는 성능과 파일 크기 면에서 사실상 별 차이가 없으므로 이 부분은 너무 중요하게 여기지 않아도 됩니다).

함수형 컴포넌트의 주요 단점은 state와 라이프사이클 API의 사용이 불가능하다는 점인데요. 이 단점은 리액트 v16.8 업데이트 이후 Hooks라는 기능이 도입되면서 해결되었습니다. 완전히 클래스형 컴포넌트와 똑같이 사용할 수 있는 것은 아니지만 조금 다른 방식으로 비슷한 작업을 할 수 있게 되었습니다. 이번 장에서 Hooks에 대한 내용은 맛보기로만 조금 배워 보고, 8장에서 더 자세히 다루겠습니다.



```javascript
function()을 사용했을 때는 검둥이가 나타나고, () =>를 사용했을 때는 흰둥이가 나타납니다. 일반 함수는 자신이 종속된 객체를 this로 가리키며, 화살표 함수는 자신이 종속된 인스턴스를 가리킵니다.

화살표 함수는 값을 연산하여 바로 반환해야 할 때 사용하면 가독성을 높일 수 있습니다.

function twice(value) {
  return value * 2;
}

const triple = (value) => value * 3;
```



### props

props는 properties를 줄인 표현으로 컴포넌트 속성을 설정할 때 사용하는 요소입니다. props 값은 해당 컴포넌트를 불러와 사용하는 부모 컴포넌트(현 상황에서는 App 컴포넌트가 부모 컴포넌트입니다)에서 설정할 수 있습니다.

MyComponent.js, App.js

```javascript
import React from 'react';
import PropTypes from ‘prop-types‘;

const MyComponent = props => {
  return (
    <div>
      안녕하세요, 제 이름은 {props.name}입니다. <br />
      children 값은 {props.children}
      입니다.
    </div>
  );
};
 
MyComponent.defaultProps = {
  name: '기본 이름'
};
 
export default MyComponent;

//props 없이 사용하는방법
import React from 'react';
 
const MyComponent = props => {
  const { name, children } = props;
  return (
    <div>
      안녕하세요, 제 이름은 {name}입니다. <br />
      children 값은 {children}
      입니다.
    </div>
  );
};
//또는
const MyComponent = ({ name, favoriteNumber, children }) => {
  return (
    <div>
      안녕하세요, 제 이름은 {name}입니다. <br />
      children 값은 {children}
      입니다.
      <br />
      제가 좋아하는 숫자는 {favoriteNumber}입니다.
    </div>
  );
};
//클래스형

class MyComponent extends Component {
static defaultProps = {
        name: ‘기본 이름‘
    };
static propTypes = {
    name: PropTypes.string,
    favoriteNumber: PropTypes.number.isRequired
}; //내부에 선언 가능

  render() {
    const { name, favoriteNumber, children } = this.props; // 비구조화 할당
    return (
      <div>
        안녕하세요, 제 이름은 {name}입니다. <br />
        children 값은 {children}
        입니다.
        <br />
        제가 좋아하는 숫자는 {favoriteNumber}입니다.
      </div>
    );
  }
}


MyComponent.defaultProps = {
  name: '기본 이름'
};
 
export default MyComponent;

MyComponent.propTypes = {
  name: PropTypes.string
  favoriteNumber: PropTypes.number.isRequired //필수
}; //props의 타입 지정


import React from 'react';
import MyComponent from './MyComponent';

const App = () => {
  return (
    <MyComponent name=“React“ favoriteNumber={1}>
      리액트
    </MyComponent>
  );
};

export default App;
```



propTypes종류

• array: 배열

• arrayOf(다른 PropType): 특정 PropType으로 이루어진 배열을 의미합니다. 예를 들어 arrayOf(PropTypes.number)는 숫자로 이루어진 배열입니다.

• bool: true 혹은 false 값

• func: 함수

• number: 숫자

• object: 객체

• string: 문자열

• symbol: ES6의 Symbol

• node: 렌더링할 수 있는 모든 것(숫자, 문자열, 혹은 JSX 코드. children도 node PropType입니다.)

• instanceOf(클래스): 특정 클래스의 인스턴스(예: instanceOf(MyClass))

• oneOf(['dog', 'cat']): 주어진 배열 요소 중 값 하나

• oneOfType([React.PropTypes.string, PropTypes.number]): 주어진 배열 안의 종류 중 하나

• objectOf(React.PropTypes.number): 객체의 모든 키 값이 인자로 주어진 PropType인 객체

• shape({ name: PropTypes.string, num: PropTypes.number }): 주어진 스키마를 가진 객체

• any: 아무 종류