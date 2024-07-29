- 최소한의 사용자 인터페이스 기능
### Alert
- `alert()`형태의 함수를 실행하면 사용자가 OK버튼을 누를 때까지 메시지를 보여주는 창을 띄움
``` js
alert("Hello");
```
- Alert창이 떠있는 동안 사용자는 다른 페이지와의 상호작용이 불가능
### Prompt
- 브라우저에서 제공하는 prompt 함수는 두 개의 인수를 받는다.
```js
result = prompt(title, [default]);
// 대괄호([])는 선택
```
- 함수가 실행되면 텍스트 메시지와 입력 필드(input field), `확인(OK)`, 및 `취소(Cancel)`버튼이 있는 모달 창을 띄워준다.
- `취소` 혹은 `ESC`를 입력하면 `null`을 반환한다.
#### title
- 사용자에게 보여줄 문자열
##### default
- 입력 필드의 초깃값 (선택값)
#### EX
```js
let result = prompt("나이를 입력해주세요", 100);
alert(`당신의 나이는 ${result}입니다.`);
```

## Confirm
- 매개변수로 받은 질문과 확인 및 취소 버튼이 있는 모달찰을 보여준다.
- 확인 버튼을 누르면 `true`, 취소 버튼을 누르면 `false`를 반환한다.
```js
let result = confirm("질문");
alert(result); // 확인을 누르면 true
```
