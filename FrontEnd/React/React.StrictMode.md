- [[Effect]]를 작성하며 useEffect를 사용하다가 `useEffect` 안에 있는 `consol.log`가 두번 찍히는 현상이 발생하여 원인을 찾아 보다 `React.StrictMode`의 원인이라는 것을 찾고 이를 정리해 보았다.
## React.StrictMode이란?
- 애플리케이션의 잠재적인 문제를 알아내기 위한 도구
- 자손 컴포넌트의 부가적인 검사와 경고를 한다.
``` jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
	<React.StrictMode> // 여기
		<App />
	</React.StrictMode>
);
```
- `index.js` 파일을 보면 `React.StrictMode`에 App 컴포넌트가 들어가 있는 것을 확인할 수 있다.
- `React.StrictMode`는 개발 모드에서만 활성화 된다.
## 함수가 2번 출력되는 이유
- 렌더링 단계의 여러 생명주기 메서드들이 여러번 호출될 수 있기 때문에, 메모리 누수와 같은 부작용이 없는것이 중요하다.
- Strict모드가 부작용을 찾아줄 수는 없지만, **일부 함수를 이중으로 호출**하여 부작용을 예측하고 문제가 되는 부분을 찾을 수 있게 돕는다.
- 이중으로 호출하는 함수는 아래와 같다.
	- 클래스 컴포넌트의 `constructor`, `render` 그리고 `shouldComponentUpdate`메서드
	- 클래스 컴포넌트의 `getDerivedStateFromProps static` 메서드 함수 컴포넌트 바디
	- `State updater` 함수 (`setState`의 첫번째 인자)
	- `useState`, `useMemo `그리고 `useRenducer`에 전달되는 함수
	
- 위처럼 2중으로 호출하는 함수가 있어 `useEffect`의 함수가 2번 동작하는 것 이였다.