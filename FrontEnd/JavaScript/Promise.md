- 콜백 지옥을 해결하기 위한 문법
```js
function increaseAndPrint(n, callback) {
  setTimeout(() => {
    const increased = n + 1;
    console.log(increased);
    if (callback) {
      callback(increased); // 콜백함수 호출
    }
  }, 1000);
}
increaseAndPrint(0, n => {
  increaseAndPrint(n, n => {
    increaseAndPrint(n, n => {
      increaseAndPrint(n, n => {
        increaseAndPrint(n, n => {
          console.log('끝!');
        });
      });
    });
  });
});
```
- 위와 같은 지옥의 비동기 처리 코드를

```js
function increaseAndPrint(n) {
  return new Promise((resolve, reject)=>{
    setTimeout(() => {
      const increased = n + 1;
      console.log(increased);
      resolve(increased);
    }, 1000)
  })
}
increaseAndPrint(0)
  .then((n) => increaseAndPrint(n))
  .then((n) => increaseAndPrint(n))
  .then((n) => increaseAndPrint(n))
  .then((n) => increaseAndPrint(n)); // 체이닝 기법
```
- 프로미스를 사용하면 위와 같이 개선할 수 있다. 

## 프로미스 객체 기본 사용법
#### 1. 프로미스 객체 생성
- Promise 객체를 생성하려면 new 키워드와 Promise 생성자 함수를 사용하면 된다.
- Promise 생성자 안에 두개의 매개변수를 가진 콜백함수가 있다.
	1.  작업이 성공했을 때 성공임을 알려주는 객체
	2. 작업이 실패했을 때 실패임을 알려주는 객체
> Promise 생성자안에 들어가는 콜백 함수를 executor라고 부른다.

```js
const myPromise = new Promise((resolve, reject) => {
	// 비동기 작업 수행
    const data = fetch('서버로부터 요청할 URL');
    
    if(data)
    	resolve(data); // 만일 요청이 성공하여 데이터가 있다면
    else
    	reject("Error"); // 만일 요청이 실패하여 데이터가 없다면
})
```
- 성공하느냐 실패하느냐에 따라 매개변수를 호출하는 것이 달라짐을 볼 수 있다.
#### 2. 프로미스 객체 처리
- 위처럼 생성된 Promise 객체는 비동기 작업이 완료된 이후에 다음 작업을 연결시켜 진행할 수 있다.
- 작업 결과에 따라 `.then()`과 `.catch()` 메서드 체이닝을 통해 성공과 실패에 대한 후속 처리를 진행할 수 있다.
- 처리가 성공하여 프로미스 객체 내부에서 `resolve(data)`를 호출하게 되면 바로 `.then()`으로 이어져 then 메서드의 콜백 함수에서 성공에 대한 추가 처리를 진행
- 처리가 실패하여 `reject("Error")`를 호출하게 되면, 바로 .catch()로 이어져 catch메서드의 콜백 함수에서 에러에 대한 추가 처리를 진행한다.
```js
myPromise
    .then((value) => { // 성공적으로 수행했을 때 실행될 코드
    	console.log("Data: ", value); // 위에서 return resolve(data)의 data값이 출력된다
    })
    .catch((error) => { // 실패했을 때 실행될 코드
     	console.error(error); // 위에서 return reject("Error")의 "Error"가 출력된다
    })
    .finally(() => { // 성공하든 실패하든 무조건 실행될 코드
    	
    })
```
#### 3. 프로미스 함수 등록
- 사용할 때는 다음과 같이 사용 가능하다.
```js
// 프로미스 객체를 반환하는 함수 생성
function myPromise() {
  return new Promise((resolve, reject) => {
    if (/* 성공 조건 */) {
      resolve(/* 결과 값 */);
    } else {
      reject(/* 에러 값 */);
    }
  });
}
// 프로미스 객체를 반환하는 함수 사용
myPromise()
    .then((result) => {
      // 성공 시 실행할 콜백 함수
    })
    .catch((error) => {
      // 실패 시 실행할 콜백 함수
    });
```
## 프로미스 3가지 상태
- 프로미스는 비동기 작업의 결과를 약속하는 것이다.
- 프로미스 객체가 생성되면, 그 비동기 작업은 이미 진행 중이고 언젠가는 성공하거나 실패할 것이다.
- 이러한 진행중, 성공, 실패 상태를 나타내는 것이 **프로미스의 상태(state)** 이다.
![[Pasted image 20240802145418.png]]
#### 1. Pending 상태
- 대기(Pending) 상태로 아직 비동기 처리 로직이 완료 되지 않은 상태이다.
```js
const promise = new Promise((resolve, reject) => {
	setTimeout(() => {
    	resolve("처리 완료")
    }, 5000)
});
console.log(promise); // Pending (대기) 상태
```
#### 2. Fulfilled 상태
- 프로미스의 내부에서 resolve()가 수행되어 비동기 작업을 성공한 상태이다.
- "완료" 상태로 봐도 부방하다.
#### 3. Rejected 상태
- Fulfilled 상태와 반대로 `reject()` 함수가 호출되어 비동기 작업을 실패한 상태이다.
- 이 상태에서 `.catch()` 함수를 사용하여 error를 확인하고 후속조치를 할 수 있다.

## 프로미스 핸들러
- 프로미스의 작업이 완료되었을때, 결과 값 혹은 에러 값을 받아 후속 작업을 수행하게 해주는 함수들이다.
#### 1. then()
- 프로미스가 이행(fulfilled)되었을 때 실행할 콜백 함수를 등록하고, 새로운 프로미스를 반환한다.
#### 2. catch() 
- 프로미스가 거부(rejected) 되었을 때 실행항 콜백 함수를 등록하고, 새로운 프로미스를 반환한다.
#### 3. finally()
- 프로미스의 상태에 상관없이 실행할 콜백 함수를 등록하고 새로운 프로미스를 반환한다.

## 프로미스 체이닝
- 위에서 다룬 프로미스 핸들러를 마치 체인과 같이 연결하여 값을 처리하는 방법이다.
```js
function doSomething() {
  return new Promise((resolve, reject) => {
      resolve(100)
  });
}
doSomething()
    .then((value1) => {
        const data1 = value1 + 50;
        return data1
    })
    .then((value2) => {
        const data2 = value2 + 50;
        return data2
    })
    .then((value3) => {
        const data3 = value3 + 50;
        return data3
    })
    .then((value4) => {
        console.log(value4); // 250 출력
    })
```
- 위와 같이 `.then()`을 체이닝 하여 값을 처리하는 것을 볼 수 있다.
- 이는 프로미스 핸들러가 처리한 값을 **자동으로 프로미스 객체에 감싸 반환**하기 때문에 가능하다.

```js
new Promise((resolve, reject) => {
  throw new Error("에러 발생!");
})
    .catch(function(error) {
      console.log("에러가 잘 처리되었습니다. 정상적으로 실행이 이어집니다.");
    })
    .then(() => {
        console.log("다음 핸들러가 실행됩니다.")
    })
    .then(() => {
        console.log("다음 핸들러가 또 실행됩니다.")
    });

// 출력
// 에러가 잘 처리되었습니다. 정상적으로 실행이 이어집니다.
// 다음 핸들러가 실행됩니다.
// 다음 핸들러가 또 실행됩니다.
```
- 만일 catch 핸들러 다음으로 then 핸들러가 이어서 체이닝 되어 있다면, 에러가 처리되고 가까운 then 핸들러로 제어 흐름이 넘어가 실행이 이어진다.

#### 참고
---
- https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EB%B9%84%EB%8F%99%EA%B8%B0%EC%B2%98%EB%A6%AC-Promise