- TS에서는 ts파일끼리 글로벌 모듈로 import, export를 안해도 자동을 가져가 쓸 수 있다.
- 이걸 가능하게 하는 파일이 d.ts 파일이다.

### d.ts 파일이란?
- definition의 약자인 d이다.
- 자바스크립트로 컴파일 되지 않는다는 특징이 있다.
- 타입정의만 따로 저장해놓고 import해서 쓸때나 타입을 정리해놓을 래퍼런스용으로 사용한다.

### TS 파일의 특징
- .ts 파일들은 신기하게 export나 import를 하지 않아도 서로의 변수나 함수를 사용할 수 있다.
- `index.ts`에서 작성한 `a`라는 변수를 `hello.ts`에서 사용 가능하다는 것이다.
- 여기서 이런 특징을 `ambient module`이라 한다.
- 근데 만약 여기서 어느 곳이든 export나 import를 사용하게 된다면, 공유가 불가능한 `local module` 상태가 된다.

### d.ts을 쓰는 곳
- 주로 써드파티라이브러리에서 타입을 지정해야 할때 사용한다.
- 아무 써드파티라이브러리의 Type을 타고 들어가면 `.d.ts` 확장자 파일로 연결되어 있다는 것을 알 수 있다.

### d.ts 파일을 reference용으로 사용하기
- tsconfig에 가서 아래 옵션을 true로 설정하면, ts파일 저장시 파일 옆에 d.ts 파일이 생성되고 타입만 쭉 정리되어 있다.
```ts
{
    "compilerOptions": {
        "target": "es5",
        "module": "es6",
        "declaration": true, // true로 변경
    }
}
```

### 참고
---
https://velog.io/@cmk0905/Typescript-d.ts-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0