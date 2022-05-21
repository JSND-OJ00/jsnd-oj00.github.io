# React LIfecycle

React Official - [State and Lifecycle](https://ko.reactjs.org/docs/state-and-lifecycle.html)

이하 [w3schools](https://www.w3schools.com/react/react_lifecycle.asp#:~:text=Each%20component%20in%20React%20has,Mounting%2C%20Updating%2C%20and%20Unmounting.) 번역

## Mounting

mounting이란, DOM에 element를 추가하는 것입니다.

리엑트에는 컴포넌트를 마운팅할 때 다음과 같은 순서로  built-in 메서드가 호출됩니다.

1. constructor()
2. getDerivedStateFromProps()
3. render()
4. componentDidMount()

### constructor

컨스트럭터 메소드는 컴포넌트가  초기화 되면 가장 먼저 호출되며, 초기 state와 다른 초기값들이 주어집니다.

컨스트럭터는 props를 인수로 받이 호출되며, 가장 먼저 super(props)로 시작해야 합니다. 이로써 부모 컴포넌트의 컨스트럭터 메서드를 초기화하며, 해당 컴포넌트가 부모 컴포넌트의 메서드를 상속하게 됩니다.

```jsx
class Header extends React.Component {
  constructor(props) {
    super(props);
    this.state = {favoritecolor: "red"};
  }
  render() {
    return (
      <h1>My Favorite Color is {this.state.favoritecolor}</h1>
    );
  }
}

ReactDOM.render(<Header />, document.getElementById('root'));
```

### getDerivedStateFromProps

이 메서드는 DOM의 엘리먼트가 렌더링되기 직전에 호출됩니다.

state와 props를 인수로 받아 props를 바탕으로 변경된 state 객체를 리턴합니다.

아래 예제는, favorite color가 red인 상태로 사직되지만, getDerivedStateFromProps() 메서드가 propsdml favcol 속성을 바탕으로 favorite color를 업데이트 합니다.

```jsx
class Header extends React.Component {
  constructor(props) {
    super(props);
    this.state = {favoritecolor: "red"};
  }
  static getDerivedStateFromProps(props, state) {
    return {favoritecolor: props.favcol };
  }
  render() {
    return (
      <h1>My Favorite Color is {this.state.favoritecolor}</h1>
    );
  }
}

ReactDOM.render(<Header favcol="yellow"/>, document.getElementById('root'));
```

### render

이 메서드는 반드시 필요하며, 실질적으로 DOM에 HTML을 내보내는 역할을 합니다

```jsx
class Header extends React.Component {
  render() {
    return (
      <h1>This is the content of the Header component</h1>
    );
  }
}

ReactDOM.render(<Header />, document.getElementById('root')); 
```

### componentDidMount

이 메서드는 컴포넌트가 렌더링된 다음에 호출됩니다.

이미 DOM에 위치한 컴포넌트를 필요로하는 작업을 수행합니다.

```jsx
// 처음 favorite color는 red였지만, 이제 yellow입니다.
class Header extends React.Component {
  constructor(props) {
    super(props);
    this.state = {favoritecolor: "red"};
  }
  componentDidMount() {
    setTimeout(() => {
      this.setState({favoritecolor: "yellow"})
    }, 1000)
  }
  render() {
    return (
      <h1>My Favorite Color is {this.state.favoritecolor}</h1>
    );
  }
}

ReactDOM.render(<Header />, document.getElementById('root'));
```

## Updating

컴포넌트는 state혹은 props에 변화가 생길 때 마다 업데이트도빈다.

리엑트에서는 컴포넌트가 업데이트 되면 아래 메서드들이 다음과 같은 순서로 호출됩니다.

1. getDeriverdStateFromProps()
2. shouldComponentUpdate()
3. Render()
4. getSnapshotBeforeUpdate()
5. componentDidUptate()

render()만 필수이며, 나머지는 정의되었을 경우에만 호출됩니다.

### getDerivedStateFromProps

아래예제에서 button은 favorite color를 blue로 바꾸는 역할을 하지만, props의 favcol을 바탕으로 업데이트하는 getDerivedStateFromProps가 호출되어 아직까지 favorite color는 yellow로 렌더링됩니다.

```jsx
class Header extends React.Component {
  constructor(props) {
    super(props);
    this.state = {favoritecolor: "red"};
  }
  static getDerivedStateFromProps(props, state) {
    return {favoritecolor: props.favcol };
  }
  changeColor = () => {
    this.setState({favoritecolor: "blue"});
  }
  render() {
    return (
      <div>
      <h1>My Favorite Color is {this.state.favoritecolor}</h1>
      <button type="button" onClick={this.changeColor}>Change color</button>
      </div>
    );
  }
}

ReactDOM.render(<Header favcol="yellow"/>, document.getElementById('root'));
```

### shouldCmponentUpdate

이 메서든느 리엑트가 렌더링을 계속해야 할지 말아야 할지를 정하기 위한 Boolean을 리턴합니다.

기본값은 true입니다.

아래 예제는 이 메서드가 false를 리턴하는 경우입니다.

따라서 렌더링이 중단됩니다.

```jsx
class Header extends React.Component {
  constructor(props) {
    super(props);
    this.state = {favoritecolor: "red"};
  }
  shouldComponentUpdate() {
    return false;
  }
  changeColor = () => {
    this.setState({favoritecolor: "blue"});
  }
  render() {
    return (
      <div>
      <h1>My Favorite Color is {this.state.favoritecolor}</h1>
      <button type="button" onClick={this.changeColor}>Change color</button>
      </div>
    );
  }
}

ReactDOM.render(<Header />, document.getElementById('root'));
```

### Render()

컴포넌트를 다시 렌더링합니다.

### getSnapshotBeforeUpdate

이 메서드를 이용하면 업데이트되기 전의 props 혹은 state에 접근할 수 있습니다. 즉, 업데이트 후, 업데이트전의 value를 확인할 수 있습니다.

이 메서드가 존재한다면 componentDidUpdate역시 포함되어야 합니다. 그렇지 않으면 error가 발생합니다.

아래 예제에서는, 우선, favorite color “red”로 렌더링된 컴포넌트가 마운팅됩니다.

다음으로, 타이머가 state를 변경, 1초가 지나면 favorite color는 yellow가 됩니다. 이로 인하여 컴포넌트는 update phase에 접어들게 되며, getSnapshotBeforeUpdate()가 정의되어 있기 때문에 실행되고, 빈 DIV1 엘리먼트에 메시지가 접힙니다.

그리고, componentDidUpdate()메서드가 실행되어 빈 DIV2 엘리먼트에 메시지가 접힙니다.

```jsx
class Header extends React.Component {
  constructor(props) {
    super(props);
    this.state = {favoritecolor: "red"};
  }
  componentDidMount() {
    setTimeout(() => {
      this.setState({favoritecolor: "yellow"})
    }, 1000)
  }
  getSnapshotBeforeUpdate(prevProps, prevState) {
    document.getElementById("div1").innerHTML =
    "Before the update, the favorite was " + prevState.favoritecolor;
  }
  componentDidUpdate() {
    document.getElementById("div2").innerHTML =
    "The updated favorite is " + this.state.favoritecolor;
  }
  render() {
    return (
      <div>
        <h1>My Favorite Color is {this.state.favoritecolor}</h1>
        <div id="div1"></div>
        <div id="div2"></div>
      </div>
    );
  }
}

ReactDOM.render(<Header />, document.getElementById('root'));
```

### componentDidUpdate

마운팅 당시 favorite color = red

타이머가 color yellow로

위의 행동으로 update phase진입, componentDidUpdate 메서드가 존재하기 때문에, 이가 실행되고, 빈 DIV 엘리먼트에 메시지가 적힘.

```jsx
class Header extends React.Component {
  constructor(props) {
    super(props);
    this.state = {favoritecolor: "red"};
  }
  componentDidMount() {
    setTimeout(() => {
      this.setState({favoritecolor: "yellow"})
    }, 1000)
  }
  componentDidUpdate() {
    document.getElementById("mydiv").innerHTML =
    "The updated favorite is " + this.state.favoritecolor;
  }
  render() {
    return (
      <div>
      <h1>My Favorite Color is {this.state.favoritecolor}</h1>
      <div id="mydiv"></div>
      </div>
    );
  }
}

ReactDOM.render(<Header />, document.getElementById('root'));
```

## Unmounting

### componentWillUnmount

이 메서드는 컴포넌트가 DOM으로부터 제거되기 전에 호출됩니다.

```jsx
class Container extends React.Component {
  constructor(props) {
    super(props);
    this.state = {show: true};
  }
  delHeader = () => {
    this.setState({show: false});
  }
  render() {
    let myheader;
    if (this.state.show) {
      myheader = <Child />;
    };
    return (
      <div>
      {myheader}
      <button type="button" onClick={this.delHeader}>Delete Header</button>
      </div>
    );
  }
}

class Child extends React.Component {
  componentWillUnmount() {
    alert("The component named Header is about to be unmounted.");
  }
  render() {
    return (
      <h1>Hello World!</h1>
    );
  }
}

ReactDOM.render(<Container />, document.getElementById('root'));
```
