- 동기와 비동기의 기본적인 개념은 [링크](https://won-percent.tistory.com/150) 참조

### 비동기의 병렬 처리 원리
![](images/Pasted%20image%2020250120181717.png)
자바 스크립트는 시이글 스레드 언어이지만 작업들을 동시에 처리 가능한 이유는 Web API 때문이다. Web API는 서버에게 리소스를 요청하거나 파일 입출력 혹은 타이머 대기 작업을 실행하는 멀티 스레드이다. 이는 브로우저마다 다르겠지만, 크롬의 경우 멀티 스레드로 구현되어 있다.

## 비동기 처리를 위한 구조
브라우저는 웹 사이트 화면을 보여주기 위해 각각의 역할이 있는 부품들로 구성되어 있다.

![](images/Pasted%20image%2020250121111041.png)
브라우저의 구성요소:
- **CallStack**: 자바스크립트 엔진이 코드를 실행을 위해 사용하는 메모리 구조
- **Heap**: 동적으로 생성된 자바스크립트 객체가 저장되는 곳
- **Web APIs**: 브라우저에서 제공하는 API 모음으로, 비동기적으로 실행되는 작업들을 전담하여 처리함 (AJAX 호출, 타이머 함수, DOM 조작 등)
- **Callback Queue**: 비동기적 작업이 완료되면 실행되는 함수들이 대기하는 공간
- **Event Loop**: 비동기 함수들을 적절한 시점에 싱행시키는 관리자
- **Event Table**: 특정 이벤트(timeOut, Click, Mouse 등)가 발생했을 때 어떤 callback 함수가 호출되야 하는지 알고 있는 자료구조
### 이벤트 루프
싱글 스레드인 자바스크립트의 작업 순서를 컨트롤하기 위한 시스템이다. 이벤트 루프는 브라우저 내부의 Call Stack, Callback Queue, Web APIs 등의 요소를 **모니터**링하면서 비동기적으로 실행되는 작업들을 관리하고, 이를 순서대로 처리하여 프로그램의 **흐름을 제어**한다.

### 이벤트 루프의 동작 과정
이벤트 루프는 자바스크립트의 `setTimeout`이나 `fetch`와 같은 비동기 자바스크립트 코드를 브라우저 Web APIs에 맡기고, 백그라운드 작업이 끝난 결과를 콜백 함수 형태로 **큐(CallBack Queue)** 에 넣고 처리 준비가 되면 **호출 스택(Call Stack)** 에 넣어 마무리 작업을 한다.

![](images/Pasted%20image%2020250120183329.png)

### Web APIs의 종류
Web APIs는 타이머, 네트워크 요청, 파일 입출력, 이벤트 처리 등 브라우저에서 제공하는 다양한 API를 총칭하는 것이다. Web API는 브라우저에서 멀티 스레드로 구현되어 있어 비동기 작업에 대해 메인 스레드를 차단하지 않고 다른 스레드를 사용할 수 있다.

Web APIs의 종류:
- **DOM**: HTML 문서의 구조와 내용을 표현하고 조작할 수 있는 객체
- **XMLHttpRequest**: 서버와 비동기적으로 데이터를 교환할 수 있는 객체 (AJAX 기술의 핵심)
- **Timer API**: 일정한 시간 간격으로 함수를 싱행하거나 지연시키는 메소드들을 제공
- **Console API**: 개발자 도구에 콘솔 기능을 제공
- **Canvas API**: `<canvas>` 요소를 통해 그래픽을 그리거나 애니메이션을 만들 수 있는 메소드들을 제공
- **Geolocation API**: 웹 브라우저에서 사용자의 현재 위치정보를 얻으 수 있는 메소드들을 제공

모든 Web API들이 비동기로 동작되는 것은 아니다. 예를들어 DOM API나 Console API는 동기적으로 처리되고, XMLHttpRequest나 Timer API는 비동기적으로 동작한다.

### Callback Queue의 종류
Callback Queue도 여러 종류의 Queue를 묶어 총칭하는 개념이다. 

종류: 
- **Task Queue**: `setTimeout`, `setInterval`, `fetch`, `addEventListener`와 같이 비동기로 처리되는 함수들의 콜백 함수가 들어가는 큐 (macrotask queue라고도 불린다.)
- **Microtask Queue**: `Promise.then`, `process.nextTick`, `MutationObserver`와 같은 우선적으로 비동기로 처리되는 함수들의 콜백 함수가 들어가는 큐 (처리 우선순위가 높음)

일반적으로 **Microtask Queue**가  **Task Queue**보다 우선순위가 높다. **Microtask Queue**의 우선순위는 브라우저를 렌더링 하는 과정보다 높기 때문에 어느정도 조심해서 사용할 필요가 있다.

### 참고
- https://inpa.tistory.com/entry/%F0%9F%94%84-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A3%A8%ED%94%84-%EA%B5%AC%EC%A1%B0-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC