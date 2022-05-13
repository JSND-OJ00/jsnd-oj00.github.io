# 220513 회고

# Tree UI

- 헤맨 이유 : 전체 구조를 잘 파악하지 못했다!
- 재귀를 사용할려면 반복되는 구조를 찾는 것이 중요

| name |  |  |  |  |
| --- | --- | --- | --- | --- |
| children |  |  |  |  |
| [ | name |  |  |  |
|  | children |  |  |  |
|  | [ | name |  |  |
|  |  | children |  |  |
|  |  |  |                          ] |  |
|  |  |  |  |                          ] |

```jsx
<ul id="root">	
	</li>
		<span>name</span>
		<input></input>
		<ul>children</ui>
	<li>
</root>
```

root 안에서 li 포함 li 내부 반복

```jsx
<ul id="root">	
	<li>
		<span>name</span>
		<input></input>
		<ul>children</ui>
			<li>
				<span>name</span>
				<input></input>
				<ul>children</ui>
			</li>
	</li>
</ul>
```

li 내부는 자식이 존재할 때만 생성

```jsx
<ul id="root">	
	<li>
	<li>
</root>
```

귀찮아서 할 지는 모르겠지만 나중에 정리한다고 치고 다로 코드로 넘어가보자.

```jsx
function createTreeView(menu, currentNode) {
  for (let i = 0; i < menu.length; i++) {
    const li = document.createElement("li");
    if (menu[i].children) {
      //input
      const input = document.createElement("input");
      input.type = "checkbox";
      //span
      const span = document.createElement("span");
      span.textContent = menu[i].name;
      //ul
      const ul = document.createElement("ul");
      //li에 다 더해줍니다
      li.append(input, span, ul);
      //root에 더해줍니다
      currentNode.append(li);
      //자식은 같은 구조를 지니고 있습니다. 자식이 없는 자식이 나올 때 까지 재귀합니다
      //자식은 root가 아닌 ul에 붙어야 합니다
      createTreeView(menu[i].children, ul);
    } else {
      li.textContent = menu[i].name;
      currentNode.append(li);
    }
  }
}
```

이것저것 적었지만 가장 중요한 것은 **전체 구조를 파악할 것**.

반복되는 부분이 보인다면 그 로직을 재귀함수로 구현하면 된다!
