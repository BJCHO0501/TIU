- Props는 부모 컴포넌트로 부터 자식 컴포넌트에 데이터를 보낼 수 있게 해주는 방법
- Properties를 줄여서 Props 라고 하는거 같다.
## 컴포넌트 구성하기
- 먼저 자식 컴포넌트와 부모 컴포넌트를 간단하게 구현해 보자.
``` javaScript
// 자식 컴포넌트
const Btn = () => {
	return (
		<button style={{
			background: "tomato",
			color: "white",
			padding: "10px 20px",
			borderRadius: "5px"
		}}>
			Save Button
		</button>
	)
}

// 부모 컴포넌트
function App() {
	<div>
		<Btn />
	</div>
}
```
- 위와같이 자식 컴포넌트인 `Btn`과 이 컴포넌트를 사용하는 `App`컴포넌트가 있을 때, 만약 `Btn`의 내부 내용이 Save Button이 아닌 다른 내용인 버튼을 만들어야 한다면, 어떻게 해야할까?
- 여기서 새로운 컴포넌트를 만들어 버린다면 코드만 길어질 것이다.
- 그래서 pros를 사용하여 컴포넌트의 내용을 변경하여 사용할 수 있다.
## Props 사용하기
``` javaScript
// 자식 컴포넌트
const Btn = (props) => {
	return (
		<button style={{
			background: "tomato",
			color: "white",
			padding: "10px 20px",
			borderRadius: "5px"
		}}>
			{props.text}
		</button>
	)
}

// 부모 컴포넌트
function App() {
	<div>
		<Btn text="Save"/>
		<Btn text="Close"/>
	</div>
}
```
- 위와 같이 컴포넌트의 props를 사용한다면, 컴포넌트를 사용하는 App에서 속성으로 컴포넌트의 내부 값을 변경할 수 있다.

- 아래는 같은 결과물의 코드 이지만, `props.{속성이름}`형태를 축약하여 사용할 수 있게 해주는 방법이다.
``` javaScript
// 자식 컴포넌트
const Btn = ({ text, big }) => { // 여러 props는 다음과 같이 사용할 수 있다.
	return (
		<button style={{
			background: "tomato",
			color: "white",
			padding: "10px 20px",
			borderRadius: "5px",
			fontSize: big ? 18 : 16
		}}>
			{text}
		</button>
	)
}

// 부모 컴포넌트
function App() {
	<div>
		<Btn text="Save" big={true}/>
		<Btn text="Close" big={false}/>
	</div>
}
```

## Props로 함수 넘기기
- 아래 코드를 보자
``` javaScript
<Btn onClick={}/>
```
- 위 코드처럼 만든 컴포넌트에 `onClick`을 사용한다면, 클릭 이벤트가 발생할까?

- 결과는 발생하지 않는다. 위와 같은 `onClick`은 그저 `Props`에 불과하다. 
- 그렇다면 어떻게 `Btn` 컴포넌트 안에있는 버튼의 클릭 이벤트를 지정할 수 있을까?
- Props로 함수를 넘겨 컴포넌트 내부에 있는 버튼의 onClick에 적용하는 방법으로 문제를 해결할 수 있다.
``` javaScript
// 자식 컴포넌트
const Btn = ({ text, onClick }) => {
	return (
		<button style={{
			background: "tomato",
			color: "white",
			padding: "10px 20px",
			borderRadius: "5px"
			onClick={onClick} // 받아온 함수 사용
		}}>
			{text}
		</button>
	)
}

// 부모 컴포넌트
function App() {
	const clickEvent = () => { consol.log("click!"); }
	
	return (
		<div> // onClick으로 함수를 넘겨줌
			<Btn text="Save" onClick={clickEvent} />
			<Btn text="Close"/>
		</div>
	)
}
```
- 위 코드처럼 Props로 함수를 넘기고 컴포넌트에서 실행시키면 원하는 이벤트를 실행시킬 수 있다.

## Props가 변경됐을 때만 렌더링하기 (fact. [[Memo]])
- 컴포넌트에 콘솔로그를 찍고 `useState`로 렌더링 하면, Props의 값이 바뀌지 않은 컴포넌트도 다시 렌더링 되는 것을 알 수 있다.
- 이를 방지하기 위해 리엑트에서는 Props가 변경된 컴포넌트만 렌더링 하도록 하는 `memo`라는 기능을 지원한다.
``` javaScript
// 자식 컴포넌트
const Btn = ({ text, onClick }) => {
	consol.log(text, "is render");

	return (
		<button style={{
			background: "tomato",
			color: "white",
			padding: "10px 20px",
			borderRadius: "5px"
			onClick={onClick}
		}}>
			{text}
		</button>
	)
}

// 부모 컴포넌트
function App() {
	const [btnContent, setBtnContent] = React.useState("defalt");
	const changeContent = () => setBtnContent("Changed!");

	const MemoBtn = React.memo(Btn);
	
	return (
		<div>
			<MemoBtn text=btnContent onClick={changeContent} />
			<MemoBtn text="hello"/>
		</div>
	)
}
```
- 위처럼 `memo`를 적용한 컴포넌트를 사용하면, Props가 변경된 컴포넌트만 리렌더링 한다.
- 컴포넌트가 많은 상황에서 리렌더링을 할 때 성능적 이점을 얻을 수 있다.