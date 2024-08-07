- hooking: 각종 컴퓨터 프로그램에서 소프트웨어 구성 요소 간에 발생하는 함수호출, 메시지, 이벤트 등을 중간에서 바꾸거나 갈채는 명령, 방법, 기술이나 행위를 말함.
## React Hook
- 함수형 컴포넌트에서도 클래스형 컴포넌트의 기능을 사용할 수 있게 하는 기능
- 훅을 통해 함수형 컴포넌트에서도 컴포넌트 상탯값을 관리할 수 있고, 컴포넌트의 생명주기 함수를 이용할 수 있음
### 장점
- 재사용 가능한 로직을 쉽게 만듬
- 훅은 단순한 함수이므로 함수 안에서 다른 함수를 호출하는 것으로 새로운 훅을 만들 수 있음
- 내장 훅과 다른 사람들이 만든 여러 커스텀 훅을 조립해서 쉽게 새로운 훅을 만들 수 있음
- 같은 로직을 한곳으로 모을 수 있어 가독성이 좋음
## React Hook List
#### 기본Hook
- [[State|useState]]
- [[Effect|useEffect]]
- useContext
#### 추가 Hook
- useReducer
- useCallback
- useMemo
- [[useRef]]
- useImpreativeHandle
- useLayoutEffect
- useDebugValue

## Custom Hook
- 내장훅을 이용하여 새로운 커스텀 훅을 제적하고 재사용할 수 있음
- 가독성을 위해 네이밍은 `use{훅 이름}` 를 따름
```js
import { useEffect, useState } from 'react';

const useWindowWidth = () => {
  const [width, setWidth] = useState(window.innerWidth);

  useEffect(() => {
    const onResize = () => {
      setWidth(window.innerWidth);
    };

    window.addEventListener('resize', onResize);

    return () => {
      window.removeEventListener('resize', onResize);
    };
  }, []);

  return width;
};
```
- 창의 넓이가 변경되면 새로운 창의 넓이를 제공받아 다시 랜더링 된다.
```js
function App() {
  const width = useWindowWidth();
  return <div className="App">{`Width is ${width}`}</div>;
}
```

- 위와 같이 컴포넌트 내부의 로직을 훅으로 분리하여 가독성을 향상 시킬 수 있다.

## Hook 사용규칙
1. 하나의 컴포넌트에서 훅을 호출하는 순서는 항상 같아야 한다. -> 내부적으로 훅 처리를 호출된 순서로 관리한다.
2. 함수형 컴포넌트, 커스텀 훅 안에서만 호출되어야 한다.

#### 참고
---
- https://k-developer.gitbook.io/dev/react/react-hook