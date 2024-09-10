> TypeScript의 타입 호환성은 구조적 서브타이핑을 기반으로 한다. 구조적 타이핑이란 오직 맴버만으로 타입을 관계시키는 방식이다. 명목적 타이핑과는 대조적이다. TypeScript의 구조적 타입 시스템의 기본 규칙은 y가 최소한 x와 동일한 맴버를 가지고 있다면 x와 y는 호환된다는 것이다.

### 1. 타입 호환성의 필요
- 아래와 같이 `Food` 타입의 객체를 인자로 받아 칼로리를 구하는 `calculateCalorie` 함수가 있다고 하자.
![](image/Pasted%20image%2020240909112427.png)

``` ts
type Food = {
  /** 각 영양소에 대한 gram 중량값 */
  protein: number;
  carbohydrates: number;
  fat: number;
}

function calculateCalorie(food: Food){
  return food.protein * 4
    + food.carbohydrates * 4
    + food.fat * 9
}
```

- 만약 calculateCalorie 함수 인자에 여러가지 타입의 객체를 전달한다 가정할때, TS의 타입 시스템은 프로그램이 타입 오류를 일으킬 가능성을 검사하게 된다.
![](image/Pasted%20image%2020240909112721.png)
- (1)의 경우, Food 타입과 동일하기 때문에 문제가 생기지 않는다.
- (2)는 완전히 다른 타입이기 때문에 오류로 판단한다.
- 그렇다면 (3)은 어떤식으로 판단하는 것이 좋을까?
```ts
type Burger = Food & {
  /** 햄버거 브랜드 이름 */
  burgerBrand: string;
}
```
- 만약 `Burger` 타입이 위와같이 `Food` 타입을 상속하고 있다면, 이는 오류가 아닌 것일까?

- TS의 타입 시스템은 유연성을 위해 부분적으로 타입 호환을 지원하고 있다. 이를 위해 어떠한 경우에 호환을 허용할 것인지에 대한 명확한 규칙이 필요하다.
- 이러한 규칙 중 프로그래밍 언어들에서 널리 활용되는 방식으로 **명목적 서브타이핑(nomainal subtyping)** 과 **구조적 서브타이핑(structural subtyping)** 이 있다.
#### 1-1. 명목적 서브 타이핑
- 명목적 서브타이핑은 타입 정의 시에 **상속 관계**임을 명확히 명시한 경우에만 타입 호환을 허용하는 것이다. 
- 이 방법을 통해 타입 오류가 발생할 가능성을 배제하고, 개발자의 명확한 의도를 반영할 수 있다.
```ts
/** 상속 관계 명시 */
type Burger = Food & {
  burgerBrand: string;
}

const burger: Burger = {
  protein: 29,
  carbohydrates: 48,
  fat: 13,
  burgerBrand: '버거킹'
}

const calorie = calculateCalorie(burger)
/** 타입검사결과 : 오류없음 (OK) */
```
#### 1-2. 구조적 서브 타이핑
- 구조적 서브타이핑은 상속 관계가 명시되어 있지 않더라도 **객체의 프로퍼티를 기반**으로 사용처에서 사용함에 문제가 없다면 타입 호환을 허용하는 방식이다.
- 아래의 `burger`의 변수의 경우 `Food` 타입의 프로퍼티를 모두 포함하고 있고 따라서 `calculateCalorie` 함수 실행 과정에서 오류가 발생하지 않는다.
```ts
const burger = {
  // Food의 파입 프로퍼티를 모두 포함하고 있음
  protein: 29,
  carbohydrates: 48,
  fat: 13,

  burgerBrand: '버거킹'
}

const calorie = calculateCalorie(burger)
/** 타입검사결과 : 오류없음 (OK) */
```
- 구조적 서브타이핑은 타입 시스템이 객체의 프로퍼티를 체크하는 과정을 수행해주므로, 명목적 서브타이핑과 동일한 효과를 내면서 개발자가 일일이 상속 관계를 명시해주어야 하는 수고를 덜어주게 된다.

> 구조적 서브타이핑은 “만약 어떤 새가 오리처럼 걷고, 헤엄치고, 꽥꽥거리는 소리를 낸다면 나는 그 새를 오리라고 부를 것이다.” 라는 의미에서 **덕 타이핑 (duck typing)** 이라고도 한다.

- TS의 Type Checker는 구조적 서브타이핑을 기반으로 타입 호환을 판단한다.
### 2. TS의 타입 구분
- 위 구조적 서브타이핑 예시의 코드는 타입 호환성에 따라 타입 오류가 발생하지 않지만, 아래의 코드의 경우 `Argument is not assignable to parameter of type 'Food'` 라는 타입 오류가 일어나게 된다.
```ts
const calorie = calculateCalorie({
  protein: 29,
  carbohydrates: 48,
  fat: 13,
  burgerBrand: '버거킹'
})
/** 타임검사결과 : 오류 (NOT OK)*/
```
- 단순 구조적 서브타이핑 관점에서 본다면 오류가 나지 않을 상황인데, 왜 오류가 발생하는 것일까?
#### 2-1. TS 컴파일러
- 위 문제의 원인을 알아보기 위해서는 먼저 TS의 컴파일러를 알아볼 필요가 있다.
- TS 컴파일러는 TS 소스코드를 AST(Abstract Syntax Tree)로 변환한 뒤, 타입 검사를 수행한 후 JS 소스코드로 변환하는 과정을 담당한다.
- 이중, 타입 검사를 수행하는 과정은 `binder.ts, checker.ts` 파일에서 담당한다. (자세한 구조는 [여기](https://github.com/microsoft/TypeScript/tree/main/src/compiler)를 참고)

- 아래는 타입검사와 가장 연관성이 높은 checker.ts 파일의 코드중 한 부분이다.
```ts
/** 함수 매개변수에 전달된 값이 FreshLiteral인 경우 true가 됩니다. */
const isPerformingExcessPropertyChecks =
    getObjectFlags(source) & ObjectFlags.FreshLiteral;

if (isPerformingExcessPropertyChecks) {
    /** 이 경우 아래 로직이 실행되는데,
     * hasExcessProperties() 함수는
     * excess property가 있는 경우 에러를 반환하게 됩니다.
     * 즉, property가 정확히 일치하는 경우만 허용하는 것으로
     * 타입 호환을 허용하지 않는 것과 같은 의미입니다. */
    if (hasExcessProperties(source as FreshObjectLiteralType)) {
        reportError();
    }
}
/**
 * FreshLiteral이 아닌 경우 위 분기를 skip하게 되며,
 * 타입 호환을 허용하게 됩니다. */
```
- 함수에 인자로 들어온 값이 `FreshLiteral` 인지 아닌지 여부에 따라 조건분기가 발생하여 타입 호환 허용 여부가 결정된다는 것을 확인할 수 있었다. 
- 그렇다면 `FreshLiteral`은 무엇이며 왜 이 경우 타입 호환에 예외가 발생하도록 되어있을까?
#### 2-2. FreshLiteral 이란?
- TS는 구조적 서브타이핑에 기반한 타입 호환의 에외 조건과 관련하여 [[신선도(freshness) - edit|신선도(freshness)]]라는 개념을 제공한다. 
- 모든 object literal은 초기에 "fresh"하다고 간주되며, 타입 단언을 하거나 타입 추론에 의해 object literal의 타입이 확장되면 "freshness" 가 사라지게 된다.
- 특정 변수에 object literal을 할당하는 경우 이 2가지 중 한가지가 발생하게 되므로 "frechness"가 사라지게 되며, 함수에 인자로 object literal을 바로 전달하는 경우에는 "fresh"한 상태로 전달된다.
- 즉, 처음 오류가 발생한 코드는 `fresh`하기 때문에 오류가 발생한 것이다.
#### 2-3. 왜 Fresh한 Object를 허용하지 않는가?
```ts
/** 부작용 1
 * 코드를 읽는 다른 개발자가 calculateCalorie 함수가
 * burgerBrand를 사용한다고 오해할 수 있음 */
const calorie1 = calculateCalorie({
  protein: 29,
  carbohydrates: 48,
  fat: 13,
  burgerBrand: '버거킹'
})

/** 부작용 2
 * birgerBrand 라는 오타가 발생하더라도
 * excess property이기 때문에 호환에 의해 오류가
 * 발견되지 않음 */
const calorie2 = calculateCalorie({
  protein: 29,
  carbohydrates: 48,
  fat: 13,
  birgerBrand: '버거킹'
})
```
- 구조적 서브타이핑에 기반한 타입 호환은 유연함을 제공한다는 이점이 있지만, 위 사례와 같이 코드의 가독성을 해치거나, 오해를 이르키는 상황이 발생할 가능성이 있다.
- 심지어 프로퍼티 키에 대한 오타가 발생 하더라도 오류가 확인되지 않는 부작용이 있다.
- fresh object를 함수 인자로 전달한 경우, 이는 특정한 변수에 할당되지 않았으므로 **해당 함수에서만 사용되고 다른 곳에서 사용도지 않는다.**
- 이 경우 유연함에 대한 이점보다 부작용을 발생시킬 가능성이 높으므로 굳이 구조적 서브타이핑을 지원해야 할 이유가 없다.
### 3. 타입 허용, 제한하기
- 앞서 말한거 처럼, TS 컴파일러는 fresh object에 구조적 서브타이핑을 지원하지 않는다.
- 하지만, 상황에 따라 이를 가능하게 할 수도 있다.
#### 3-1. fresh object의 구조적 서브타이핑 허용하기
- 개발자가 fresh object에 대해서 타입 호환을 허용하고자 한다면 아래와 같은 함수 매개변수 타입에 index signature를 포함시켜두어 명시적으로 타입 호환을 허용시키는 것이 가능하다.
```ts
type Food = {
  protein: number;
  carbohydrates: number;
  fat: number;
  [x: string]: any                  /** index signature */
}

const calorie = calculateCalorie({
  protein: 29,
  carbohydrates: 48,
  fat: 13,
  burgerBrand: '버거킹'
})
/** 타임검사결과 : 오류없음 (OK) */

```
- 또는 tsconfig 상에 `suppressExcessPropertyErrors`를 true로 설정하는 방식도 가능하다.
#### 3-2. 모든 경우에 대해 타입 호환을 허용하지 않기
- Branded type기법을 사용하여 모든 경우에 대해 타입 호환을 허용하지 않도록 강제하는 것이 가능하다.
```ts
type Brand<K, T> = K & { __brand: T};
type Food = Brand<{
  protein: number;
  carbohydrates: number;
  fat: number;
}, 'Food'>

const burger = {
  protein: 100,
  carbohydrates: 100,
  fat: 100,
  burgerBrand: '버거킹'
}

calculateCalorie(burger)
/** 타임검사결과 : 오류 (NOT OK) */
```
- 위와 같이 의도적으로 `__brand`와 같은 프로퍼티를 추가시켜 개발자가 함수의 매개변수로 정의한 타입 외에는 호환 될 수 없도록 강제하는 기법이다.
- 온도(섭씨, 화씨)나 화폐단위(원, 달러, 유로)와 같이 `number` 타입이지만 서로 다른 의미를 가질 수 있어 명시적인 구분이 필요할 때 사용해 볼 수 있다.

### 참고
---
https://toss.tech/article/typescript-type-compatibility
https://www.typescriptlang.org/ko/docs/handbook/type-compatibility.html