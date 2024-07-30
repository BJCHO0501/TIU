- 이 문서에서는 flexbox의 구성과 속성들을 살펴볼 것이다.
- 프론트엔드에서 레이아웃을 잡는 방법
- iOS의 FlexLayout과 유사한 방법이다.
## flexBox의 구성
- flexbox는 복수의 자식 요소인 flex item과 그 상위 요소인 flex container로 구성된다.
- flexbox는 정렬하려는 요소의 부모 요소에 `display: flex` 속성을 선언하면 된다.
```css
.flex_container {
	display: flex;
}
```
- `display: flex`속성이 적용된 요소는 flex container가 되고, flex container의 자식 요소는 flex item이 된다.
- flex container과 flex item은 부모, 자식 관계

- flex item은 주축(main axis)에 따라 정렬된다. 추축의 방향은 flex container의 flex-direction 속성으로 결정한다. 기본값은 `row`이다.
- 전체적인 정렬이나 흐름에 관련된 속성은 flex container에 정의하고, 자식 요소의 크기나 순서에 관련된 속성은 flex item에 정의한다.
	- **flex container 속성**: `flex-direction`, `flex-wrap`, `justify-content`, `align-items`, `align-content`
	- **flex item 속성**: `flex`, `flex-grow`, `flex-shrink`, `flex-basis`, `order`

## Flex Container Attributes
#### Flex-direction
- flex-item의 정렬 방향을 설정하는 속성
- row, row-reverse, column, colum-reverse
- 기본값: `row`

  ![[Pasted image 20240730115247.png]]
#### flex-wrap
- flex item이 flex container을 벗어났을 때 줄을 바꾸는 속성
- nowrap, wrap, wrap-reverse
- 기본값: `nowrap`

  ![[Pasted image 20240730115841.png]]
#### justify-content
- 주축을 기준으로 flex item을 수평으로 정렬한다.
- flex-start, center, flex-end, space-around, space-between
- 기본값: `flex-start`

![[Pasted image 20240730120428.png]]
> **justify-items 속성은 제안된 스펙은 존재하지만, 공식적으로 지원하지는 않는다고 한다.**

#### align-items
- 주축을 기준으로 flex item을 수직으로 정렬한다.
- stretch, flex-start, center, baseline, flex-end
- 기본값: `stretch`
 ![[Pasted image 20240730120702.png]]
 
### align-content
- flex item이 여러줄로 나열되어 있을 때 주축을 기준으로 수직 정렬 방법을 설정하는 속성
- stretch, flex-start, center, flex-end, space-around, space-between
- 기본값: `stretch`
 ![[Pasted image 20240730121214.png]]

## Flex item Attributes
#### flex-grow
- flex item의 확장에 관련된 속성이다. 0과 양의 정수를 속성 값으로 사용한다.
- 속성값이 0이면 flex container의 크기가 거져도 flex item의 크기가 커지지 않고 원래 크기로 유지된다.
- 속성값이 1이면 원래 크기에 상관없이 flex container를 채우도록 flex item의 크기가 커진다.

![[Pasted image 20240730123112.png]]

#### flex-shrink
- flex item의 축소에 관련된 속성이다. 0과 양의 정수를 속성 값으로 사용한다. 
- 기본값: `1`
- 속성값이 0이면 flex container의 크기가 flex ite의 크기보다 작어져도 flex item의 크기가 줄어들지 않고 원래 크기로 유지된다.
- 속성값이 1이상 이면 flex container의 크기가 flex item의 크기보다 작어질 때 container에 맞춰 item의 크기가 줄어든다.
 ![[Pasted image 20240730123401.png]]

#### flex-basis
- flex item의 기본 크기를 결정하는 속성이다.
- 기본값: `auto`
- width 속성에서 사용하는 모든 단위를 사용 가능하다.
- 속성을 0으로 설정하면 flex item은 절대적이 되어 flex container을 기준으로 크기가 결정된다.
 ![[Pasted image 20240730125022.png]]
#### order
- flex item의 순서를 정하는 속성
- `order: {양수};` 의 형태로 사용하며 지정한 양수의 숫자가 작을 수록 앞으로 온다.
 ![[Pasted image 20240730181527.png]]


> 참고: https://d2.naver.com/helloworld/8540176