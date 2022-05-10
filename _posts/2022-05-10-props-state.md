# PROPS and STATES

# PROPS

1. 하워 컴포넌트에 전달하고자 하는 값(data)과 속성을 정의한다.
2. props를 이용하여 정의된 값과 속성을 전달한다.
3. 전달받은 props를 렌더링한다.

## 방법1.

부모: jsx 속성 및 값을 할당(props 생성)

```jsx
<Child attribute={value}/>
<Child text={"I'm your son"}>
```

자식: props 매개변수 사용

props는 객체.
이 객체의 { key : value }는
부모 컴포넌트에서 정의한 { attribue : value }.
따라서 아래와 같이 접근 가능.

```jsx
<p>props.value</p>
```

예

```jsx
function Parent() {
	return (
		<div className="parent">
			<h1>I'm the parent</h1>
			<Child text={"I'm the eldest child"}>
			<Child/>
		</div>
	);
};

function Child(props) {
	return (
		<div className="child">
			<p>props.text</p>
		</div>
	);
};
```

## 방법2

부모: 여는 태그와 닫는 태그 사이에 value를 넣어 전달

```jsx
<Child>I'm the eldest child</Child>
```

자식: props.children을 이용하여 해당 value에 접근

```jsx
<p>{props.children}</p>
```

예

```jsx
function Parent() {
	const text1 = '내가'
	const text2 = '네 새끼다.'
	return (
		<div className="parent">
			<h1>I'm the parent</h1>
			<Child>{text1}</Child>
			<Child>{text2}</Child>
			<Child/>
		</div>
	);
};

function Child(props) {
	return (
		<div className="child">
			{props.children}
		</div>
	);
};
```

# STATE

```jsx
const [state 저장 변수, state 갱신 함수] = useState(상태 초기 값);
```

- useState를 호출하면 배열반환
- 0번째 요소는 현재 state 변수, 1번째 요소는 이 변수를 갱신할 수 있는 함수
- useState의 인자는 state의 초기값

```jsx
import {useState} from "react";

function CheckboxExample() {
	const [isChecked, setIsChecked] = useState(false);
```

- isChecked : state를 저장하는 변수
- setIsChecked : state  isChecked 를 변경하는 함수
- useState : state hook
- false : state 초기값

ex)

```jsx
function CheckboxExample() {
const [ischecked, setIsChecked] = useState(false);

const handleChecked = (event) => {
	setIsChecked(event.target.checked);
};

return (
		<div className="App">
			<input type="checkbox" checked={isChecked} onChange={handleCheked} />
			<span>{isChecked? "Checked" : "UnChecked"}</span>
		</div>
	);
}
```