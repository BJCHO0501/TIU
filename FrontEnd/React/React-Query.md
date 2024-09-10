> fetching, caching, 서버 데이터와의 동기화를 지원해주는 라이브러리

- React Query는 서버 데이터의 관리를 보다 편안하게 해주는 라이브러리로, 다음과 같은 다양한 기능을 제공한다.
### 1. 캐싱
- React-Query는 데이터를 캐싱한다.
- 따라서 개발자가 따로 캐싱 작업을 하지 않아도 캐싱을 해주기 때문에 서버의 부하를 덜 수 있다.
#### 1-1. 최신 데이터 구분법
- 그렇다면 어떤 식으로 캐싱된 데이터가 최신 데이터인지 구별 가능할까?
- react에서는 최신 데이터를 fresh, 기존의 데이터를 stale한 데이터라 표현한다.
- 그리고 이 fresh와 stable한 데이터를 관리하기 위해 아래와 같은 시점에 적절히 데이터를 갱신해준다. 
	1. 화면을 보고 있을 때
	2. 페이지의 전환이 일어났을 때
	3. 페이지 전환 없이 이벤트가 발생해 데이터를 요청할 때

- 이를 위해 React-Query에서는 기본적인 옵션을 제공한다.
```ts
refetchOnWindowFocus, //default: true
refetchOnMount, //default: true
refetchOnReconnect, //default: true
staleTime, //default: 0
cacheTime, //default: 5분 (60 * 5 * 1000)
```

`refetchOnWindowFocus`: 브라우저에 포커스가 들어온 경우
`refetchOnMount`: 새로운 컴포넌트 마운트가 발생한 경우
`refetchOnReconnect`: 네트워크 재연결이 발생한 경우
- 위 3가지는 Refetch 트리거라 할 수 있다.

`staleTime`
- 데이터가 fresh -> stale 상태로 변경되는데 걸리는 시간이다.
- fresh 상태에서는 refetch가 일어나도 데이터가 변경되지 않는다. 
- 기본값이 0일때는 refetch가 일어날때 마다 데이터가 갱신된다.

`cacheTime`
- 데이터가 inactive한 상태일 때 캐싱된 상태로 남아있는 시간이다.
- 특정 컴포넌트가 unmount되면 사용된 데이터는 inactive상태로 바뀌고, 이때 데이터는 cacheTime만큼 유지된다.
- cacheTime이후에는 GC로 수집되어 메모리에서 해제된다.
- 만일 cacheTime이 지나지 않았는데 해당 데이터 컴포넌트가 다시 mount된다면, **새로운 데이터를 fetch해오는 도안 캐싱된 데이터를 보여준다.**
- fetch하는동안 **임시**로 보여준다는 점에 유의하자.
### 2. 대표적인 기능들
- 기본적으로 Get에는 useQuery가 Put, Update, Delete에는 useMutation이 사용된다.
#### 2-1. useQuery
- 첫 번째 파라미터로 unique key를 포함한 배열이 들어간다. 이는 캐싱 작업에서 사용된다.
- 첫 번째 파라미터에 들어가는 배열의 첫 요소는 unique key로 사용되고, 두 번째 요소부터는 query 함수 내부의 파라미터로 실제 호출하고자 하는 비동기 함수가 들어간다. 이때 함수는 Promise를 반환하는 형태여야 한다
- 최종 반환 값은 API의 성공, 실패 여부, 반환값을 포함한 객체이다.
```ts
import {
  QueryClient,
  QueryClientProvider,
  useQuery,
} from '@tanstack/react-query'

const queryClient = new QueryClient()

export default function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <Example />
    </QueryClientProvider>
  )
}

function Example() {
  const { isPending, error, data } = useQuery({
    queryKey: ['repoData'],
    queryFn: () =>
      fetch('https://api.github.com/repos/tannerlinsley/react-query').then(
        (res) => res.json(),
      ),
  })

  if (isPending) return 'Loading...'

  if (error) return 'An error has occurred: ' + error.message

  return (
    <div>
      <h1>{data.name}</h1>
      <p>{data.description}</p>
      <strong>👀 {data.subscribers_count}</strong>{' '}
      <strong>✨ {data.stargazers_count}</strong>{' '}
      <strong>🍴 {data.forks_count}</strong>
    </div>
  )
}
```
- 위 코드를 보면, `isPending`을 통해 로딩여부, `error`을 통해 에러 발생 여부, `data`를 통해 성공 시 데이터를 반환할 수 있다.
#### 2-2. useQueries
- 여러개의 useQuery를 한 번에 실행하고자 하는 경우, Promise.all()처럼 묶어 실행할 수 있도록 도와준다.
```ts
const ids = [1, 2, 3]
const results = useQueries({
  queries: ids.map((id) => ({
    queryKey: ['post', id],
    queryFn: () => fetchPost(id),
    staleTime: Infinity,
  })),
})

const ids = [1, 2, 3]
const combinedQueries = useQueries({ 
  queries: ids.map((id) => ({
    queryKey: ['post', id],
    queryFn: () => fetchPost(id),
  })),
  combine: (results) => {
    return {
      data: results.map((result) => result.data),
      pending: results.some((result) => result.isPending),
    }
  },
})
```
#### 2-3. useMutation
- Put, Update, Delete와 같이 값을 변경할 때 사용하는 API이다.
- 반환값은 useQuery와 동일하다.
```ts
function App() {
  const mutation = useMutation({
    mutationFn: (newTodo) => {
      return axios.post('/todos', newTodo)
    },
  })

  return (
    <div>
      {mutation.isLoading ? (
        'Adding todo...'
      ) : (
        <>
          {mutation.isError ? (
            <div>An error occurred: {mutation.error.message}</div>
          ) : null}

          {mutation.isSuccess ? <div>Todo added!</div> : null}

          <button
            onClick={() => {
              mutation.mutate({ id: new Date(), title: 'Do Laundry' })
            }}
          >
            Create Todo
          </button>
        </>
      )}
    </div>
  )
}
```
- useQuery와 다른점은 post 비동기 함수를 넣었다는 것과 두 번째 인자로 상황별 분기 설정이 들어간다는 차이점이 있다.
- 실제 사용시에 mutate 메서드를 사용하고, 첫 번째 인자로 API 호출 시에 전달해주어야 하는 데이터를 넣어주면 된다.

#### 참고
---
https://velog.io/@kandy1002/React-Query-%ED%91%B9-%EC%B0%8D%EC%96%B4%EB%A8%B9%EA%B8%B0