- CallBack Queue는 Web API가 수행한 비동기 함수를 넘겨받아 Event Loop가 해당 함수를 Call Stack에 넘겨줄 때까지 비동기 함수들을 쌓아놓는 곳이다.
![](images/Pasted%20image%2020250120183904.png)
- CallBack Queue에는 `Task Queue`과 `Microtask Queue` 2가지 종류가 있다.
### Task Queue (Macrotask Queue)
- Task Queue는 다른 말로 Macrotask Queue라고도 부른다.
- Macrotask Queue는 아래와 같은 task를 넘겨 받는다.
	- setTimeout()
	- setInterval()
	- setImmediate()

### Microtask Queue
- Macrotask Queue와 다르게 Microtask Queue는 아래와 같은 비동기 호출을 넘겨 받는다.
	- Promise
	- async/await
	- process.nextTick
	- Object.observe
	- MutationObserver
- 또 일반 Task나 Macrotask Queue보다 우선순위가 높다는 특징이 있다.

![](https://res.cloudinary.com/practicaldev/image/fetch/s--05Fi8vBq--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/42eatw03fcha0e1qcrf0.gif)

#### 참고
- https://inpa.tistory.com/entry/%F0%9F%8C%90-js-async
- https://inpa.tistory.com/entry/%F0%9F%94%84-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A3%A8%ED%94%84-%EA%B5%AC%EC%A1%B0-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC