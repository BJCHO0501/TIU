- TypeScript는 구조적으로 타입 호환성이 있는 객체 타입을 쉽게 검사할 수 있도록 신선도라는 개념을 제공한다. 
``` ts
function logName(something: { name: string }) {
    console.log(something.name);
}

var person = { name: 'matt', job: 'being awesome' };
var animal = { name: 'cow', diet: 'vegan, but has milk of own species' };
var random = { note: `I don't have a name property` };

logName(person); // 오케이
logName(animal); // 오케이
logName(random); // 오류: 속성 `name` 누락
```

- 하지만 구조적 타입 처리는 무언가가(함수의 매개변수 등..) 실제 다루는 것보다 더 많은 데이터를 받아들인다는 오해를 불러 이르킬 수 있다. 이런 경우는 아래 코드와 같이 TypeScript의 오류로 확인할 수 있다.
``` ts
function logName(something: { name: string }) {
    console.log(something.name);
}

logName({ name: 'matt' }); // 오케이
logName({ name: 'matt', job: 'being awesome' }); // 오류: 객체 리터럴은 정의된 속성만 지정해야 함. 여기서 `job`은 불필요.
```
- 만약 위와같은 오류가 발생하지 않는다면, 코드를 보는 다른 사람은 logName이라는 함수가 실제로 name에 대한 처리만 진행하지만 job에 대한 처리도 진행한다 오해할 수 있다.

- 또 선택적(optional) 맴버가 있는 경우에, 오타가 발생한다면 그냥 넘어가는 문제가 발생할 수 있다.
``` ts
function logIfHasName(something: { name?: string }) {
    if (something.name) {
        console.log(something.name);
    }
}
var person = { name: 'matt', job: 'being awesome' };
var animal = { name: 'cow', diet: 'vegan, but has milk of own species' };

logIfHasName(person); // 오케이
logIfHasName(animal); // 오케이
logIfHasName({neme: 'I just misspelled name to neme'}); // 오류: 객체 리터럴은 정의된 속성만 지정해야 함. 여기서 `neme`은 불필요.
```
- 객체 리너럴일 때만 이런 식의 타입 검사가 수행되는 이유는 속성이 추가로 입력되었지만 그 속성이 **실제로 사용되지 않는다면** 거의 항상 오타가 발생한 경우이거나 API를 잘못 이해한 경우이기 때문이다.

## 추가 속성 허용
- 타입 선언에 인덱스 서명을 포함시키면 그 타입이 추가 속성을 허용함을 명시적으로 나타낼 수 있다.
``` ts
var x: { foo: number, [x: string]: unknown };
x = { foo: 1, baz: 2 };  // 오케이, `baz`는 인덱스 서명 부분에 해당하게 됨
```

## 결론
- TS에서는 구조적 타입 처리를 사용하는데, 이때 신선도 라는 개념을 사용하여 사용자가 발생 시킬수 있는 오류를 한번 더 체크해준다.