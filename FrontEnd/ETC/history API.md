- 브라우저가 관리하는 세션 히스토리(session history), 즉 **페이지 방문 이력을 제어하기 위한 웹 표준 API**이다.
- 보통 사용자가 새 창이나 탭을 열 때 생성되고 해당 창이나 탭을 닫을 때 소멸한다.
```js
history.length; // 1
window.history.length; // 1
document.history.length; // 1
```
- 브라우저에서 페이지를 계속 들어간 후 콘솔에 위와 같이 입력하면, 얼마나 많은 페이지를 방문하였는지 알 수 있다.

```js
history.back(); // 뒤로 가기
history.forward(); // 앞으로 가기
history.go(-2); // 뒤로 2번 가기
history.go(-1); // 뒤로 1번 가기
history.go(0); // 새로 고침
history.go(1); // 앞으로 1번 가기
history.go(2); // 앞으로 2번 가기
```
- 위와 같은 코드로 페이지간 이동도 가능하다.

#### 참고
---
- https://www.daleseo.com/js-history-api/