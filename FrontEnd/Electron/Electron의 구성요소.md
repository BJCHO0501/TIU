![](images/Pasted%20image%2020240807183510.png)
- electron의 핵심 구성요소로는 Chromium과 Node.js가 있다.
- Chromium은 웹 페이지를 렌더링하는 엔진으로 Electron에서 프론트엔드 인터페이스를 구현한다. 즉, 웹 브라우저에서 볼 수 있는 모든 시각적 요소들을 만들고 조작하는 데 사용한다.
- Node.js는 백엔드 개발에 주로 사용되는 환경으로, Electron내에서 Node.js를 통해 개발자는 사용자의 컴퓨터와 상호작용하는 데스크탑 애플리케이션을 만들 수 있다.
- 즉 Electron을 사용하면, **웹 개발자들은 별도의 네이티브 애플리케이션 개발 언어를 배우지 않고도, 단일 코드베이스로 Windows, macOS, Linux 등 다양한 운영 체제에서 실행되는 애플리케이션을 만들 수 있다.**

## Electron 애플리케이션의 작동 원리
---
![](images/Pasted%20image%2020240807184619.png)
- Electron 애플리케이션은 메인 프로세스와 렌더러 프로세스 간의 효율적인 상호 작용에 의해 동작한다.
### Main Process
- 메인 프로세스는 Electron 앱의 뇌이다.
- GUI표시, 애플리케이션 이벤트 관리, 글로벌 단축키 등록, 네이티브 메뉴와 대화 상자 생성 등 애플리케이션의 주요 기능을 처리한다.
- 메인 프로세스는 애플리케이션의 진입점이 되는 `package.json`의 메인 스크립트로 정의된다.
- 메인 프로세스는 운영체제의 네이티브 GUI 기능과 직접 상호 작용하여 애플리케이션의 창(웹 페이지들을 표시하는 BrowserWindow 인스턴스)을 생성한다.
- 메인 프로세스는 Electron 애플리케이션에 **단 하나만 존재**하며, **모든 렌더러 프로세스를 관리하는 핵심 역할**을 수행한다.

### Reanderer Process
- 렌더러 프로세스는 애플리케이션 UI를  구현하는 곳이다.
- 애플리케이션의 '화면' 역할을 하며, Chromium 기반으로 동작하고 각 Browser Window 인스턴스가 생성할 때마다 별도의 렌더러 프로세스가 실행된다.
- HTML, CSS, JS같은 웹 기술을 사용하여 인터페이스를 구성한다.
- **메인 프로세스와는 독립적으로 동작**한다.
- 메인 프로세스와 데이터를 교환(IPC 통신)하며, 이를 통해 렌더러 프로세스가 메인 프로세스에 작업을 요청하거나 데이터를 전달 받을 수 있다. (렌더러 to 렌터러는 불가능)

## 프로세스 간 통신 (IPC 통신)
---
- 메인 프로세스와 렌더러 프로세스간의 통신은 주로 IPC(Inter-Process Communication) 모듈을 사용하여 이루어 진다. 비동기, 동기 방식을 모두 지원한다.
### 비동기 IPC
- 비동기 IPC는 ipcMain과 ipcRenderer 모듈을 사용하여 구현된다.
- 메인 프로세스에서는 ipcMain을 사용하여 렌더러 프로세스로부터 메시지를 수신하고, 렌더러 프로세스에서는 ipcRenderer를 사용하여 메인 프로세스로 메시지를 전송한다.
```js
// 메인 프로세스에서
const { ipcMain } = require('electron');
ipcMain.on('asynchronous-message', (event, arg) => {
	console.log(arg); // "ping" 출력
	event.reply('asynchronous-reply', 'pong');
})

// 렌더러 프로세스에서
const { ipcRenderer } = require('electron');
ipcRenderer.send('asynchronous-message', 'ping');
ipcRenderer.on('asynchronous-reply', (event, arg) => {
	console.log(arg); // "pong" 메시지 출력
})
```
### 동기 IPC
- 동기 IPC는 렌더러 프로세스가 메인 프로세스로부터 응답을 받을 때까지 기다린다.
- 이는 `ipcRenderer.sendSync` 메서드를 사용하여 구현 되며, 사용시 렌더러 프로세스의 작업을 일시 중지시킬 수 있어 애플리케이션의 반응성에 영향을 줄 수 있다.

### remote 모듈 사용
- remote 모둘은 렌더러 프로세스에서 메인 프로세스의 기능을 직접 호출할 수 있게 해주지만, 이는 내부적으로 동기 IPC를 사용하여 구현된다. 따라서 remote 모듈의 사용은 메인 프로세스의 성능 저하를 일으킬 수 있으며, Electron 9 이후 버전에서 보안상의 이유로 기본적으로 비활성화 되어 있다. 
- 필요한 경우에만 remote 모듈을 활성화하고 신중하게 사용해야 한다.
#### 참고
---
- https://berom.tistory.com/563