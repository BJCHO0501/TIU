- `null`과 `undefined`, 숫자 `0`을 구분 지어 다뤄야 할 때 이 차이점은 매우 중요한 역할을 한다.
- nullish 별합 연산자 `??`를 사용하면 짧은 문법으로 여러 피연산자 중 그 값이 '확정되어있는' 변수를 찾을 . 수있다.
- `a ?? b`의 평가 결과는 다음과 같다.
	- `a`가 null도 아니고 `undefined`도 아니면 `a`
	- 그 외의 경우는 `b`
``` js
let firstName = null;
let lastName = null;
let nickName = "Hello!";

alert(firstName ?? lastName ?? nickName ?? "익명의 사용자"); // Hello!
```
## || 와 ??의 차이
- `||` 연산자는 값의 `boolean`여부에 따라 구별한다.
- 반면 `??`연산자는 값의 **존재여부**에 따라 판단하기 때문에 아래와 같은 상황에서의 결과물이 다르다.
```js
const hight = 0;

alert(hight || 100); // 100
alert(hight ?? 100); // 0
```