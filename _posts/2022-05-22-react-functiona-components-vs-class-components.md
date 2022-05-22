# 220522

# React-Functional Components vs Class Components

리엑트 라이프사이클을 공부하다가 문듯 의문이 들었다. 리엑트 등과 같은 라이브러리를 쓰는 목적이 코드를 쉽고 간결하게 만들어 개발과 유지보수를 용이하게 하기 위함이 아닌가? 그렇다면 복잡하게 component를 class로 만들 필요가 있을까? 우선 functional component와 class component를 비교해보자.

| Functional Components | Class Components |
| --- | --- |
| props를 인자로 받아 react element를 반환하는 plain javascript 함수 | react 라이브러리를 이용. react element는 class내 render() 메서드가 반환 |
| render() 메서드 없음 | JSX를 반환하는 render()가 반드시 필요 |
| 단순히 데이터를 받아 특정한 형태로 보여주기만 하므로, stateless. 주와 UI 렌더링만 담당. | logic과 state가 제공되므로 stateful |
| react lyfecycle 메서드가 사용될 수 없음 | class내에서 react lyfecycle 메서드 사용 가능 |
| stateful하게 만들기 위해 hooks를 간단하게 이용할 수 있음(ex. const [name, setname]) = useState(’’) | hooks를 이용하기 위해 classs내에서 다른 syntax를 사용.(ex. constructor(props) {super(props); this.state = {name:’’})}) |
| constructor 사용되지 않음 | state를 저장하기 위해 constructor가 필요 |

결론부터 적자면, functional component가 앞으로 더 많이 쓰일 것 같다.

두 방식의 예제를 찾아보면 알겠지만, functional component가 짧고 간결하며, 이는 개발하기 쉽고, 이해하기 쉬우며, 테스트 역시 용이함을 뜻한다. class를 사용하면 this를 만이 사용하게 될 것이며, 이는 코드를 혼란스럽게 만든다.

또한 react team이 class components를 대체할 수 있도록 functional component를 위한 더 많은 리엑스 혹을 지원하려고 노력하고 있다는 사실을 명심하자.

그렇다고 class component를 등한시하지말자. class component의 lyfecyle 메서드는 리엑트의 동작 원리를 이해하는데 분명 도움을 줄 것이다. 알아둬서 손해볼 건 없을 듯? 언제 또 사용하게 될지도 모를 일이고.
