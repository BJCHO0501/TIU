- 개발을 진행하다 보면 url에 따라 다른 페이지를 보여주고 싶을 때가 있다.
- 그럴때 React-Router-Dom을 사용하여 원하는 페이지를 쉽게 보여줄 수 있다.

## 라우팅이란?
- 라우팅은 네트워크에서 경로를 선택하는 프로세스를 의미한다.
- 그리고 리액트에서 라우팅이란 url에 따라 페이지를 선택하는 과정이라 볼 수 있다.
![[Pasted image 20240801144518.png]]
> 출처: 생활코딩

## React-Router를 사용하는 이유
- HTML에는 `a`라는 태그를 사용해서 페이지를 이동하는 것이 가능하다.
- 하지만 `a`태그를 사용하면 전체 페이지가 새로 로딩된다. 그로인해 깜박이는 현상이 발생하게 된다.
- [[SPA - edit]] 사용자 경험을 향상시키는데 목적이 있다.
## Roter의 종류
#### HashRouter
- URL의 해쉬(#)값을 이용하는 라우터
- 검색 엔진이 읽지 못한다.
- 별도의 서버 설정을 하지 않더라도 새로고침 시 오류가 발생하지 않는다. 이는 해쉬 라우터가 해쉬 뒤의 값은 브라우저에서만 관리하고 서버는 기본 url로 데이터를 요청하기 때문이다.
- [[history API - edit]]를 사용하지 않기 때문에 동적 페이지에 불리하다.
#### BrowserRouter
- history API를 사용한다.
- 별도의 서버 설정이 없다면 새로 고침시 404에러가 발생한다.
- 큰 프로젝트에 적합하다.

## React-Router-Dom의 모듈들
#### BrowserRouter
- history API를 활용해 history 객체를 생성한다.
- history API는 내부적으로 stack 자료 구조릐 형태를 띄기 때문에 사용자가 방문한 url 기록들을 차곡차곡 쌓는 형태로 저장해 둔다.
- 라우팅을 진행할 컴포넌트 상위에 BrowserRouter 컴포넌트를 생성하고 감싸주어야 한다.
#### Route
- 현재 브라우저의 location(window.href.location 정보) 상태에 따라 다른 element를 렌더링 한다.
- props
	- element: 조건이 맞을 때 렌더링할 element (<Element /> 형식으로 전달)
	- path: 현재 path값과 해당 값이 일치하는지 확인하고 element를 렌터링
#### Routes
- 모든 Route의 상위 경로에 존재해야 하며, location 변경 시 하위에 있는 모든 Route를 조회해 현재 locairon과 맞는 Route를 찾아준다.

## React-Router-Dom 설치
```
$ cd router-tutorial
$ yarn add react-router-dom
```
- 리엑트 돔 설치 후, 아래와 같이 라우터 모듈을 만들어 준다.
```jsx
const Router = () => {
  return (
    <BrowserRouter>
        <Routes>
          <Route path="/" element={<GalleryPage />} />
          <Route path="/gallery" element={<DetailCardPage />}>
            <Route path=":cardId" element={<DetailCard />} />
          </Route>
        </Routes>
    </BrowserRouter>
  );
};
```

```jsx
import Router from "./Router";

const App = () => (
  <>
      <Router />
  </>
);

export default App;
```

## Link
- 라우터 내에서 직접적으로 페이지 이동을 하고자 할 때 사용되는 컴포넌트이다.
```jsx
import React from 'react';
import {Link} from 'react-router-dom';

function Nav(){
  return (
    <div>
      <Link to='/'> Home </Link>
      <Link to='/about'> About </Link>
    </div>
  );
}

export default Nav;
```
- `to` 속성에 경로를 넣어주는 방식으로 사용한다.
- Link 컴포넌트는 다음 2가지와 같은 특징이 있다.
	1. Relative (상대적)
		- 계층 구조가 상대적이다.
		- 상대 경로 표현이 가능하므로, `./..` 과 같은 표현도 가능하다.
	2. preventScrollReset
		- 페이지 중간에 있는 컨텐츠 내부에서 tap 목록을 누르는 것과 같은 시도를 할 때, 기존의 Link 컴포넌트 였다면 클릭 시 스크롤이 초기화 되어 페이지 가장 위로 이동하게 된다.
	3. state
		- useLocation 훅과 연계하여 특정 state를 넘겨주는 것도 가능하다.
- Link는 a태그로 이루어져 있으나 부과적인 기능(자체적으로 컴포넌트 상태 유지, 화면 전체 리렌더링 방지 등)이 포함된 a태그의 상위 버전인 느낌이다.

## useNavigate
- 만약 다른 이벤트(버튼 클릭, 특정 함수 작동 등..)로 화면을 이동하고 싶다면 어떻게 해야할까?
- 그럴때 `useNavigate`를 사용할 수 있다.
```jsx
import { useNavigate } from "react-router-dom";

const navigate = useNavigate();

const onClick = () => {
	navigate('/')
}
```
- 다음과 같은 부과 기능들이 있다.

1. replace
	- 이동후 뒤로가기 여부를 설정할 수 있다.
	- 기본값은 `false`이다.
```jsx
navigate("/", { replace: true });
```

2. state
	- link와 마찬가지로 state를 전달해 줄 수 있다.
```jsx
navigate("/", { state: { cardId: cardId } });

const location = useLocation();
const { cardId } = location.state;
```

## 중첩 라우팅
- 특정 페이지 내에 하위 페이지를 만들 수 있는 기능이다.
- 특정 페이지 내에서 하위 페이지를 만들 수 있고, 해당 페이지마다 경로를 이용한 데이터 전달도 가능하다.
```jsx
<Route path="/about" element={<About />}>
  <Route path="location" element={<Location />}></Route>
</Route>
```
- 위 코드는 중첩 라우팅을 사용한 코드이다.
	- `/about`의 하위에 `/location`이라는 하위 라우팅이 되었다고 판단
	- 따라서 `/about/location`으로 주소를 이동할 경우 주어진 Location 컴포넌트가 렌더링 되는 것
- 하지만 위 코드만 작성한다면 하위 페이지를 어디에 렌더링 할건지 명시하지 않아 아무런 변화도 일어나지 않는다.
- 그래서 사용하는 것이 `Outlet` 컴포넌트다.
#### Outlet
```jsx
import { Outlet } from 'react-router-dom';

function About() {
  return (
    <div>
      <div>
        <h2>여기는 About 페이지입니다.</h2>
        <p>대충 쇼핑몰 페이지라는 뜻</p>
      </div>
      <Outlet /> // Location 컴포넌트가 렌더링 됨
    </div>
  );
}
```
- 위 코드처럼 상위 라우터의 컴포넌트(About) 내부에 하위 라우터의 컴포넌트(Location)가 나올 곳에 Outlet 컴포넌트를 사용하면, 라우터는 이를 인식하고 하위 컴포넌트(Location)를 렌더링하게 된다.

## RouterProvider과 CreateBrowserRouter를 사용한 라우팅
- 지금까지 위에서 설명한 라우팅 방식 말고도 `React Router v6.4`에 추가된 라우팅 방식이 있다.
- RouterProvider과 CreateBrowserRouter를 사용한 라우팅 방식으로, 기존의 jsx로 짜던 라우터 구조를 object로 짤 수 있게 해준다.
```jsx
// jsx를 사용한 구조
import { Route, Routes } from "react-router-dom";
import WeatherInfo from "./pages/weatherInfo/WeatherInfo";
import WeatherSelect from "./pages/weatherSelect/WeatherSelect";
import NotFound from "./pages/notFound/NotFound";

function App() {
    return (
        <Routes>
            <Route
                path="/"
                exact={true}
                element={<WeatherSelect />}
            />
            <Route
                path="/detail/:location"
                element={<WeatherInfo />}
            />
            <Route
                path="*"
                element={<NotFound />}
            />
        </Routes>
    );
}

export default App;
```

``` jsx
// createBrowserRouter를 사용한 구조
import { createBrowserRouter } from "react-router-dom";
import WeatherSelect from "./pages/weatherSelect/WeatherSelect";
import WeatherInfo from "./pages/weatherInfo/WeatherInfo";
import NotFound from "./pages/notFound/NotFound";

const Router = createBrowserRouter([
	{
		path: "/",
		element: <WeatherSelect />,
		errorElement: <NotFound />,
	},
	{
		path: "/detail/:location",
		element: <WeatherInfo />,
	},

]);

export default Router;
```
- 위 Router컴포넌트는 RouterProvider을 사용하여 라우팅 할 수 있다.
```jsx
import { RouterProvider } from "react-router-dom";
import Router from "./router";

function App() {
	return (
		<>
			<RouterProvider router={Router} /> // 한줄로 라우팅 적용이 가능하다.
		</>
	);
}

export default App;
```
- 아래는 좀더 복잡한 예시이다.
```jsx
const router = createBrowserRouter([
  {
    path: "/",
    element: <Root />,
    children: [
      {
        path: "dashboard",
        element: <Dashboard />,
      },
      {
        path: "about",
        element: <About />,
      },
    ],
  },
]);
```
- children을 사용해 중첩 라우팅도 간단하게 구현 가능하다.
## useParams
- url 파라미터를 조회할 때 사용한다.
```jsx
import React from 'react';
import { useParams } from 'react-router-dom';

const NewId = () => {
  let { id } = useParams();

  return (
    <div className="test">
      <p>현재 유저의 아이디는 { id } 입니다.</p>
    </div>
  )
}

export default NewId;
```
- 라우팅에 path에 : 다음으로 작성한 문장을 가져온다.(ex path="/location/`:loc` 면 loc로 가져와야 함 )
## useSearchParams
- 현재 위치에 대한 url의 쿼리스트링을 읽고 수정할때 사용한다.
```js
const [serchParams, setSearchParams] = useSearchParams();
```
- useState와 유사하게 사용한다.

#### 값을 읽어올 때
- `searchParams.get(key)` : key의 value를 가져온다. 해당 key의 value가 두 개 이상이라면 처음의 값을 반환한다.
- `searchParams.getAll(key)` : 특정 key에 해당하는 모든 value를 가져오는 메서드
#### 값을 변경할 때
- `searchParams.set(key, value)` : 인자로 전달한 key 값을 value로 설정한다. 만일 기존에 key에 대한 값이 존재했다면 덮어씌운다.
- `searchParams.append(key, value)` : 기존 값을 변경 혹은 삭제하지 않고 추가한다.

- 만일 실제 url에 반영하고 싶다면 setSearchParams에 serchParams을 인자로 전달해 주면 된다.
``` js
setSearchParams(serchParams);
```