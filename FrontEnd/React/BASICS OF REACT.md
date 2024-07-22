## 리액트를 사용하지 않을때..

```html
// index.html
<!DOCTYPE html>
	<body>
	    <h1 id='title'>count: 0</h1>
	    <button id='btn'>click</button>
	    <script>
	        let count = 0;
	
	        const title = document.getElementById('title');
	        const clickButton = document.getElementById('btn');
	
	        clickButton.addEventListener("click", () => {
	            count = count + 1;
	            title.innerText = `count: ${count}`;
	        });
	    </script>
	</body>
</html>
```

- 위와 같이 document에 직접 접근하여 요소를 가져오고, 수정하는 코드들이 생기게 되고, 많은 요소들을 수정할 때, 코드 관리가 힘들어지고 성능도 안좋아 진다.
- 그래서 React를 사용한다.

## 리엑트는 뭐가 다른가?

```jsx
const span = React.createElement(
	"span",
	{ id: "fun-span" },
	"i'm span!"
);
ReactDOM.render(span, root);
```

- 위 코드처럼, html을 작성하여 요소를 추가하는 전 방식과 달리 js를 사용하여 요소의 종류, 속성, 내용을 작성하고 랜더링 할 수 있다.
- 아래와 같이 복잡했던 과정을 간결하게 만들 수 있다.

| 리액트 사용 여부 |                       결과                        |
| :-------: | :---------------------------------------------: |
|  사용하기 전   | html의 요소를 js로 불러옴 → 불러온 요소를 js로 조작함 → html로 보여줌 |
|   사용한 후   |          js로 html의 요소를 생성 → html로 보여줌           |

- 결론적으로 기존의 복잡했던 과정을 단축하는 이점 때문에 React를 사용한다고 볼 수 있다.

## 리엑트에서의 Event 제어

```jsx
clickButton.addEventListener("click", () => {
    count = count + 1;
    title.innerText = `count: ${count}`;
});
```

- 위 바닐라 js에서 이벤트 처리를 했던 코드를 리엑트에서는 속성을 사용하여 제어할 수 있다.

```jsx
const span = React.createElement(
	"span",
	{ 
		id: "fun-span",
		onClick: () => console.log("click!"),
	},
	"i'm span!"
);
```

- onClick 속성을 사용하면 addEventListener을 사용하던 코드를 획기적으로 줄일 수 있다.

## JSX란?

- 위 챕터에서, 예시로 작성한 코드를 쓸 일이 없을 것이라 하였다.
- 왜냐하면, 리엑트를 사용할 때 createElement 코드를 작성하는 것도 길기 때문에, html과 유사한 방식으로 코드를 작성할 수 있기 때문이다.
- 그리고 이것을 가능하게 하는 것이 JSX이다.

```jsx
// not use JSX
const span = React.createElement(
	"span",
	{ 
		id: "fun-span",
		onClick: () => console.log("click!"),
	},
	"i'm span!"
);

// use JSX
const span = (
	<span id="fun-span" onClick= () => console.log("click!")>
		i'm span
	<span/>
);
```

- JSX를 사용하면 HTML과 유사하게 코드를 작성할 수 있어 간결한 코드 작성이 가능해진다.
- 그리고 이런 변환은 babel을 사용한다.