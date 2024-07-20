-  사용자에게 보여주는 화면에서의 변수값이 바뀌었을때, 이를 실제로 화면에서 바뀌게 하려면 다시 렌더링을 진행해야 한다.
- 여기서 바뀌는 변수를 **상태**라고 할 수 있다.
- 이런 상태를 관리하기 위해 리엑트에서는 아래와 같은 방법을 지원한다.
## UseState
- React에서 지원하는 상태관리 방법이다.
- 배열로 변수와 이 변수를 업데이트 할 수 있는 함수를 반환한다.
``` javaScript
const [count, modifire] = React.useState(10);
```
- 위와 같이 사용하여 count와 이를 업데이트 할 수 있는 modifire을 사용할 수 있다.

```javaScript
const [count, setCount] = React.useState(10);
// ... code

function updateCount() {
	setCount(count + 1);
}
```
- 위처럼 setCount를 사용한다면, React에서 count의 값을 바꾸고 리렌더링까지 알아서 해준다
#### 현재 상태값을 사용하여 상태를 변경할 때...
- 위 코드의 `setCount(count + 1);`을 본다면 현재 상태인 count를 사용하여 상태를 업데이트 하는 것을 볼 수 있다.
- 물론 정상적으로 동작하지만, 예상치 못한 결과를 만들 수도 있기 때문에 더 안전한 방법을 제공한다.
``` javaScript
setCount((current) -> current + 1);
```
- 위와 같이 current 함수를 사용하면 현재 상태의 값을 안전하게 불러올 수 있다.