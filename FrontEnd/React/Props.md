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