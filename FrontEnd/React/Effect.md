- 리엑트를 사용하여 코드를 작성할 때, 특정 코드는 한번만 혹은 어떤 상태가 변경 되었을 때만 작동해야 하는 상황이 있다.
- 그럴때 사용하는 것이 `useEffect`이다.
## useEffect를 사용하는 이유
- 아래 코드를 봐보자
```jsx
function App() {
	const [counter, setCounter] = useState(0);
	const clickButton = () => setCounter((pre) => pre + 1);

	console.log("i'm call :)");

	return (
		<div>
			<h1>{counter}</h1>
			<button onClick={clickButton}>click me!</button>
		</div>
	);
}
```
- 정상적인 코드라면([[React.StrictMode]]가 비활성화 되어있다는 가정) `i'm call :)`이 렌더링 될때마다 출력될 것 이다. 
- 만약 저 코드가 API를 호출하는 코드라면, 매 렌더링 즉 버튼을 눌러 state를 바꿀때마다 호출되는 문제가 발생할 것이다.
- 이런 문제를 해결하기 위해 **`원하는 시점(원하는 state가 변경)에 특정 코드를 동작`** 하게 하는 기능인 `useEffect`를 사용한다.
## useEffect 사용
```js
function useEffect(effect: EffectCallback, deps?: DependencyList): void;
```
- `useEffect`는 2개의 매개변수를 받는다.
	1. `effect`: 원하는 시점에 작동시킬 함수
	2. `deps`: 변했을 때 작동할 state들의 배열
- 만약 `deps` 가 빈 배열이라면 `useEffect`의 코드는 처음 1번만 작동한다.
##### 특정 코드 1번만 실행시키기
```jsx
function App() {
	const [counter, setCounter] = useState(0);
	const clickButton = () => setCounter((pre) => pre + 1);

	useEffect(() => {
		console.log("i'm call once");
	}, []);

	return (
		<div>
			<h1>{counter}</h1>
			<button onClick={clickButton}>click me!</button>
		</div>
	);
}
```
- 위와 같이 `useEffect`의 `deps`를 빈배열로 넣으면 내부의 함수를 1번만 실행시킨다.
##### 특정 state가 변경되었을 때만 실행시키기
```jsx
function App() {
	const [counter, setCounter] = useState(0);
	const [content, setContent] = useState("");
	const clickButton = () => setCounter((pre) => pre + 1);
	const contentOnchange = (e) => {
		setContent(e.target.value);
	}
  

	return (
		<div>
			<input value={content} onChange={contentOnchange} />
			<h1>{counter}</h1>
			<button onClick={clickButton}>click me!</button>
		</div>
	);
}
```
- 위와 같은 코드에서, input이 변경되어 content가 변경되었을 때와 버튼을 눌렀을 때 실행되는 함수를 구분하려면 어떻게 해야할까?
- `deps`의 값이 다른 `useEffect`를 만들어 주면 해결할 수 있다.
```jsx
function App() {
	const [counter, setCounter] = useState(0);
	const [content, setContent] = useState("");
	const clickButton = () => setCounter((pre) => pre + 1);
	const contentOnchange = (e) => {
		setContent(e.target.value);
	}

	// counter이 변경되었을 때 실행
	useEffect(() => {
		console.log("i's call counter change");
	}, [counter])

	// content가 변경되었을 때 실행
	useEffect(() => {
		console.log("i's call content change");
	}, [content])

	return (
		<div>
			<input value={content} onChange={contentOnchange} />
			<h1>{counter}</h1>
			<button onClick={clickButton}>click me!</button>
		</div>
	);
}
```
- 위와 같이 `counter`과 `content`의 변경에 따라 실행할 코드를 분리할 수 있다.

## CleanUp 함수
- 우리가 만든 컴포넌트가 처음 생성되었을 때, `useEffect`를 사용하여 처음 1번 함수를 실행 시킬 수 있었다. 
- 그렇다면 컴포넌트가 사라질때는 어떤 식으로 관리할 수 있을까?
- `useEffect`의 `clean-up` 함수를 사용하면 가능하다.
```jsx
const Hello = () => {
	useEffect(() => {
		console.log("HI!");
		return () => console.log("Bye!");
	}, []);
	
	return <h1>Hello!</h1>
}

function App() {
	const [isShow, setIsShow] = useState(false);
	const onClickButton = () => setIsShow((pre) => !pre);
	  
	return (
		<div>
			{isShow ? <Hello /> : null}
			<button onClick={onClickButton}>{isShow ? "hide" : "show"}</button>
		</div>
	);
}
```
 - 위와 같은 코드가 있을 때, `Hello` 컴포넌트의 `HI!`는 버튼을 눌러 보여질때 한번 실행될 것이다.
 - `useEffect`의 함수에 `return` 값으로 함수를 반환해 주면 해당 컴포넌트가 사라질때 실행된다.
 - 그래서 버튼을 눌러 컴포넌트를 지우게 된다면 `Bye!`가 출력된다.