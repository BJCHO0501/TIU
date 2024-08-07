- 렌더링에 필요하지 않은 값을 참조할 수 있는 React Hook이다.
```js
const ref = useRef(initialValue)
```
#### 매개변수
- `initialValue`: `ref` 객체의 `current` 프로퍼티 초기 설정값이다. 여기에는 어떤 유형의 값이든 지정할 수 있다. 이 인자는 초기 렌더링 이후부터는 무시된다.
#### 반환값
- `useRef`는 단을 프로퍼티를 가진 객체를 반환한다.
	- `current`: 처음 전달한 `initialValue`로 설정된다. 나중에 다른 값으로 바꿀 수 있다.
	- ref 객체를 JSX노드의 ref 어트리뷰트로 React에 전달하면 React는 current프로퍼티를 설정한다.
- 다음 렌더링에서 `useRef`는 동일한 객체를 반환한다.

- ref.current 프로퍼티를 변경해도 React는 **컴포넌트를 다시 렌더링하지 않는다.**

### ref로 값 참조하기
- 컴포넌트의 최상위 레벨에서 useRef를 호출한다.
```js
import { useRef } from 'react';  

function Stopwatch() {  
	const intervalRef = useRef(0);  
// ...
```
- ref 내부의 값을 업데이트하여면 current 프로퍼티를 수동으로 변경해야 한다.
```js
function handleStartClick() {  
	const intervalId = setInterval(() => {  
		// ...  
	}, 1000);  
	intervalRef.current = intervalId;  
}
```

- ref를 사용하면 다음을 보장한다.
	- (렌더링할 때마다 재설정되는 일반 변수와 달리) **리렌더링 사이에 정보를 저장**할 수 있다.
	- (리렌더링을 촉발하는 state 변수와 달리) 변경해도 **리렌더링을 촉발하지 않는다.**
	- (정보가 공유되는 외부 변수와 달리) 각각의 **컴포넌트에 로컬로 저장**된다.

- **렌더링 중에는 ref.current를 쓰거나 읽으면 안된다.** 예상치 못한 결과가 도출될 수도 있다.
### ref로 DOM 조작하기
- 초기값이 `null`인 ref객체를 선언한다.
```js
import { useRef } from 'react';  

function MyComponent() {  
	const inputRef = useRef(null);  
	// ...
```

- ref 객체를 ref 속성ㅇ로 조작하려는 DOM 노드의 JSX에 전달한다.
```js
// ...  
return <input ref={inputRef} />;
```

- React가 DOM 노드를 생성하고 화면에 그린 후,React는 ref 객체의 **`current 프로퍼티`를 DOM 노드로 설정**한다.
- 이제 DOM 노트 `<input>`에 접근해 `focus()`와 같은 메서드를 호출할 수 있다.

```js
function handleClick() {  
	inputRef.current.focus();  
}
```

#### 참조
---
- https://ko.react.dev/reference/react/useRef