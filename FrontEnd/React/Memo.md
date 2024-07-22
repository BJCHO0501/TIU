- `memo`는 리엑트에서 컴포넌트를 렌더링 할 때, UI 성능을 증가시키기 위해 렌터링 결과를 메모이징 함으로써,불필요한 렌더링을 건너뛰게 하는 기능이다.
- 처음 봤을 때 그냥 모든 컴포넌트에 붙이면 좋을 것이라 생각했는데, 이게 아니여서 어떤 상황에 써야 좋은지 찾아 보았다.
## Memo의 작동 방식
- React는 먼저 컴포넌트를 렌더링 한 뒤, 이전 렌더된 결과와 비교하여 DOM 업데이트를 결정한다.
- 렌더 결과가 이전과 다르면, DOM을 업데이트 하는 방식
- 여기서 다음 렌더링 결과롸 이전 결과의 비교는 이미 빠르지만, 어떤 상황에서는 이 과정의 속도를 좀더 높일 . 수 있다.

- 컴포넌트가 React.memo()로 래핑 될 때, React는 컴포넌트를 렌더링하고 결과를 메모리징 한다. 그래소 다음 렌더링이 일어날 때 `props`기 같다면, React는 메모이징된 내용을 재사용한다.

## 언제 memo를 써야 할까?
- memo를 사용하기 가장 좋은 케이스는 함수형 컴포넌트 같은 `props`로 자주 렌더링 될거라 예상될 때 이다.
```js
function MovieViewsRealtime({ title, releaseDate, views }) {
  return (
    <div>
      <Movie title={title} releaseDate={releaseDate} />
      Movie views: {views}
    </div>
  );
}
```
- 위와 같은 컴포넌트가 매초마다 업데이트 된다고 가정해보자.
```jsx
// Initial render
<MovieViewsRealtime
  views={0}
  title="Forrest Gump"
  releaseDate="June 23, 1994"
/>

// After 1 second, views is 10
<MovieViewsRealtime
  views={10}
  title="Forrest Gump"
  releaseDate="June 23, 1994"
/>

// After 2 seconds, views is 25
<MovieViewsRealtime
  views={25}
  title="Forrest Gump"
  releaseDate="June 23, 1994"
/>

// etc
```
- views만 변경되어 `MovieViewsRealtime`안의 `Movie` 컴포넌트는 바뀐 Props가 없지만 계속 리렌더링 된다.
- 이럴때 memo를 사용한다면 `Movie`의 불필요한 렌더링을 막을 수 있다.

```js
function MovieViewsRealtime({ title, releaseDate, views }) {
  return (
    <div>
      <MemoizedMovie title={title} releaseDate={releaseDate} />
      Movie views: {views}
    </div>
  );
}
```

## 언제 memo를 쓰면 안될까?
- 위 상황같이 지속적인 렌더링에서의 불필요한 렌더링을 막는 상황이 아니라면, memo를 사용하지 않는 것이 좋다.
> 성능 관련 변경이 잘못 적용 된다면 성능이 오히려 악화될 수 있다. `React.memo()`를 현명하게 사용하라.

- 만약 memo를 무분별하게 사용한다면 아래의 작업을 리렌더링 할때마다 수행한다.
	1. 이전 `props`와 다음 `props`의 동등 비교를 위해 비교 함수를 수행한다.
	2. 비교 함수는 거의 항상 `false`를 반환할 것이기 때문에, React는 이전 렌더링 내용과 다음 렌더링 내용을 비교할 것이다.