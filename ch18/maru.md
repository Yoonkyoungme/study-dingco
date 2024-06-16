<aside>
💡 [이번 장에서 살펴볼 내용]

- 반응형 아키텍처로 순차적 액션을 파이프라인으로 만드는 방법을 배웁니다.
- 상태 변경을 다루기 위한 기본형을 만듭니다.
- 도메인과 현실 세계의 상호작용을 위해 어니언 아키텍처를 만듭니다.
- 여러 계층에 어니언 아키텍처를 적용하는 방법을 살펴봅니다.
- 전통적인 계층형 아키텍처와 어니언 아키텍처를 비교해 봅니다.
</aside>

# 반응형 아키텍처

- 항상 반응형 아키텍처가 좋은 것이 아니라 판단해야함

- 원인과 효과가 결합한 것을 분리
- 여러 단계를 파이프라인으로 처리
- 타임라인이 유연해짐

- `반응형 아키텍처` : 순차적인 액션을 표현하는 방식을 뒤집음

- **순차적 액션 단계**에 사용

이벤트에 대한 반응으로 일어날 일을 지정하는 것

→ 원래 핸들러 함수에서 순서대로 실행되던 것을 여러 개의 핸들러에서 실행되도록 나눔

전통적인 아키텍처 : 한쪽에 뭔가를 추가하면 다른 쪽에 있는 모든 것을 변경하거나 복제해야함

감시자로 장바구니가 바뀔 때 할일을 지정

```jsx
function ValueCell(initialValue){
	var currentValue = currentValue;
	var wartchers = [];
	
	return {
		val: function() {
			return currentValue;
		}
		update: function(f) {
			var oldValue = currentValue;
			var newValue = f(oldValue);
			if(oldValue !== newValue){
				currentValue = newValue;
				forEach(watchers, function(watcher) {
					watcher(newValue);
					})
				}
			},
			addMatcher: function(f) {
				watchers.push(f);
			}
}
```

**감시자 적용**

- 핸들러 함수가 더 작아졌음
- 장바구니를 바꾸는 모든 핸들러에서 배송 아이콘 갱신 함수를 호출하지 않아도 됨

FormulaCell : 이미 있는 셀에서 파생한 셀을 만듦

감시하던 상위 셀 값이 바뀌면 FormulaCell 값이 바뀜

**핵심 포인트 : 상태를 가능한 한 안전하게 사용**

## 원인과 효과가 결합한 것을 분리

장바구니를 바꾸는 모든 UI 이벤트 핸들러에 같은 코드를 추가해야함

**→ 버튼 클릭이라는 원인과 그로인해 발생하는 배송 아이콘 갱신이라는 효과가 결합**

→ 이를 반응형 아키텍처를 적용하여 분리

문제가 없는데 분리 X

## 여러 단계를 파이프라인으로 처리

파이프라인 : 작은 액션과 계산을 조합한 하나의 액션

각 단계에서 생성된 데이터는 다음 단계의 입력값으로 사용

여러 단계가 있지만 데이터를 전달하지 않는다면 사용 X

# 어니언 아키텍처

`어니언 아키텍처` : 함수형 프로그래밍으로 현실 세계를 다루기 위한 고수준의 개념

서비스 전체를 구성하는 데 사용하기 때문에 바깥 세계와 상호작용을 하는 부분 다룸

인터렉션 계층, 도메인 계층, 언어 계층

- 인터렉션 계층
    - API 호출, DB 연동 등
- 도메인 계층
    - 비즈니스 규칙을 정의하는 계산

# 함수형 아키텍처

도메인 계층이 데이터베이스 계층에 의존하지 않는다

데이터베이스는 변경 가능하고 접근하는 모든 것을 액션으로 만든다

**도메인 로직은 모두 계산으로 만들어야 하기 때문에 데이터베이스와 분리하는 것이 중요**

→ 도메인 로직을 계산으로 만드는 것은 항상 가능

좋은 인프라보다 **좋은 도메인 강조**

장바구니가 어디에서 왔는지 모름

핸들러가 데이터베이스에서 장바구니를 가져와 도메인에 전달하는 역할

→ 인터렉션 계층에서 값을 가져오고, 도메인 계층에서 합산