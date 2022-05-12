# State-이벤트 처리

# onChange

```jsx
function NameForm() {
	const [name, setName] = useState("");

	const handleChange = (e) => {
		setName(e.target.value);
	}

	return (
		<div>
			<input type="text" value={name} onChange={handleChange}></input>
			<h1>{name}</h1>
		</div>
	)
};
```

# onClick

1. wrong case

onClick 이벤트에 함수를 바로 호출하면 컴포넌트가 렌더링 될 때 함수 자체가 아닌 호출 결과가 onClick에 적용되어 클릭되어도 아무 결과가 일어나지 않음.

```jsx
function NameForm() {
	const [name, setName] = useState("");

	const handleChange = (e) => {
		setName(e.target.value);
	}

	return (
		<div>
			<input type="text" value={name} onChange={handleChange}></input>
			<button onClick={alert(name)}>Button</button>
			<h1>{name}</h1>
		</div>
	)
};	
```

1. how to fix
- 리턴문 안에서 함수를 정의
- 리턴문 외부에서 함수를 정의 후 이벤트에 함수 자체를 전달
- 두 방법 모두 arrow function을 사용하여 함수를 정의해야 해당 컴포넌트가 가진 state에 함수들이 접근할 수 있음

```jsx
// 함수 정의하기

return (
	<div>
		...
		<button onClick={() => alert(name)}>Button</button>
		...
	</div>
	);
};

// 함수 자체를 전달하기

const handleClick = () => {
	alert(name);
};

return (
	<div>
		...
		<button onClick={handleClick}>Button</button>
		...
	</div>
	);
};
```

# <select>

사용자가 drop down  목록을 열어 그 중 한가지 옵션을 선택하면, 선택된 옵션이 state 변수에 갱신

```jsx
function Select () {
	const [choice, setChoice] = useState("a");

	const abc = ["a","b","c"]
	const options = abc.map((el) => {
		return <option value={el}>{el}</option>;
	});

	const handleAbc = (e) => {
		setChoice(e.target.value);
	};

	return (
		<div>
			<select onChange={handleAbc}>{options}</select>
			<h1>You choose "{choice}"</h1>
		</div>
	);
};
```

# Pop Up

```jsx
function PopUp () {
	const [showPopup, setShowPopup] = useState(false);

	const togglePopup = () => {
		setShowPopup((showPopup) => !showPopup)
	};

	return (
    <div className="App">
      <h1>Fix me to open Pop Up</h1>
      <button className="open" onClick={togglePopup}>
        Open me
      </button>
      {showPopup ? (
        <div className="popup">
          <div className="popup_inner">
            <h2>Success!</h2>
            <button className="close" onClick={togglePopup}>
              Close me
            </button>
          </div>
        </div>
      ) : null}
    </div>
  );
}
```