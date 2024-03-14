
# chapter 4. 액션에서 계산 빼내기

💡 **[ 이번 장에서 살펴볼 내용 ]**

-   어떻게 함수로 정보가 들어가고 나오는지 살펴보기
-   **테스트하기 쉽고 재사용성이 좋은 코드를 만들기 위한 함수형 기술**에 대해 알아보기
-   **액션에서 계산을 빼내는 방법** 배우기 </aside>

💡 **[ 테스트하기 쉬운 코드, 재사용성이 좋은 코드]** 
- DOM 업데이트와 비즈니스 규칙은 분리되어야 한다. 
- 전역변수가 없어야 한다. 
- 전역 변수에 의존하지 않아야 한다. 
- DOM을 사용할 수 있다고 가정하면 안된다. 
- 함수가 결과값을 반환해야한다.


# 📘 장바구니 구현 사항

-   shopping_cart, shopping_cart_total : 전역변수
-   장바구니에 제품을 담는 메서드
-   장바구니 제품이 바뀌었기 때문에 금액 합계 업데이트 (모든 제품값 더하기)
-   금액 합계를 반역하기 위해 DOM 업데이트

# 📘 새로운 기능 요구 사항

1.  구매 합계가 20 달러 이상이면 **무료 배송**을 해주기
2.  장바구니에 넣으면 합계가 **20달러가 넘는 제품의 구매 버튼 옆에 무료 배송 아이콘 표시**

# 📘 절차적인 방법으로 구현해보기

**update_shopping_icons**
페이지에 있는 모든 구매 버튼을 가져와 반복문 적용
무료 배송이 가능한지 확인
결정에 따라 무료 배송 아이콘을 보여주거나 보여주지 않음

**calc_cart_total**
모든 제품값 더하기
금액 합계를 반영하기 위해 DOM 업데이트
update_tax_dom → 페이지에 세금을 업데이트하기 위해 코드 추가

# 📘 테스트하기 쉽게 만들기

현재 비즈니스 규칙을 테스트하기 어려움 (세금을 계산하는 로직)
update_tax_dom 안에서는 `DOM에 출력하는 함수와 비즈니스 로직이 섞여있음`
**→** **현재 결과값을 얻을 방법이 DOM에서 값을 가져오는 방법밖에 없음**

# 📘 재사용하기 쉽게 만들기

현재 코드는 다른 개발부서에서 재사용하기 어려움 (전역변수, DOM 활용)

-   장바구니 정보를 전역변수에서 읽어오는데, 데이터베이스에서 읽어와야 하는 경우
-   결과를 보여주기 위해 DOM을 직접 바꾸고 있지만, DOM이 아닌 영수증 또는 운송장에 출력해야하는 경우


# 📘 함수에는 입력과 출력이 있습니다

`입력` : 함수가 계산을 하기 위한 외부 정보
`출력` : 함수 밖으로 나오는 정보나 어떤 동작
함수를 호출하는 이유는 **결과가 필요하기 때문**이며, **원하는 결과를 얻으려면 입력이 필요**

## ✅ 명시적 vs 암묵적

인자 : `명시적 입력`
리턴값 : `명시적 출력`
전역변수를 읽는 것 : `암묵적 입력`
전역변수를 바꾸는 것 또는 콘솔에 찍는 것 : `암묵적 출력`

함수에 암묵적 입력과 출력이 있다 → `액션` / 없다 → `계산`
암묵적 입력은 인자로 바꾸고 암묵적 출력은 리턴값으로 바꾸기
암묵적 입력과 출력을 `부수 효과(side effect)` 라고 함

# 📘 개선하기

DOM 업데이트는 함수에서 어떤 정보가 나오는 것이다 → 출력
리턴값이 아닌 출력 → 암묵적 출력
⇒ 암묵적 출력인 **DOM 업데이트와 비즈니스 규칙 분리** 제안

----------

**DOM을 사용할 수 있다고 가정하면 안됨**
우리가 만든 함수는 DOM을 직접 사용 → 암묵적 출력
암묵적 출력은 함수의 리턴값으로 바꿀 수 있음

----------

**함수가 결과값을 리턴해야함**
암묵적 출력 대신 명시적 출력을 사용하기

# 📘 액션에서 계산 빼내기

**서브루틴 추출**하기 : 리팩토링
아직 액션 상태
계산으로 바꾸기 위해 **어떤 입력과 출력이 있는지 확인**
```tsx
function calc_cart_total(){
	calc_total();
	set_cart_total_dom();
	update_shipping_icons();
	update_tax_dom();
}

function calc_total(){
	**shopping_cart_total = 0;** // 출력
	for(let i = 0; i < **shopping_cart**.length; i++) { // 입력
		const item = shopping_cart[i];
		**shopping_cart_total** += item.price; // 출력
	}

```

### ✅ 암묵적 출력 없애기

**전역 변수에 초기값을 할당 또는 재할당하는 로직을 없애기** → 암묵적 출력 없애기

```tsx
function calc_cart_total(){
	shopping_cart_total = calc_total();
	set_cart_total_dom();
	update_shipping_icons();
	update_tax_dom();
}

function calc_total() {
	let total = 0;
	for(let i = 0; i < shopping_cart.length; i++){
		const item = shopping_cart[i];
		total += item.proce;
		}
		return total;
}

```

### ✅ 암묵적 입력 없애기

전역 변수 값을 읽음 : 암묵적 입력 → 전역 변수 대신 **매개변수를 함수의 인자로 받아 사용**

```tsx
function calc_cart_total(){
	shopping_cart_total = calc_total(shopping_cart);
	set_cart_total_dom();
	update_shipping_icons();
	update_tax_dom();
}

function calc_total(cart) {
	let total = 0;
	for(let i = 0; i < cart.length; i++){
		const item = cart[i];
		total += item.proce;
		}
		return total;
}

```

# 📘 액션에서 또 다른 계산 빼내기

전역 변수를 읽는다 : 함수 안으로 데이터가 들어온다 → 입력
전역 배열을 바꾼다 : 함수 밖으로 데이터가 나간다 → 출력

```tsx
function add_item_to_cart(name, price){
	add_item(name, price);
}

function add_item(name, price){
	shopping_cart.push({
		name,
		price
	})
}

```

# 📘 계산 추출하기

1.  계산으로 만들 코드를 선택하고 새로운 함수로 만들기
2.  새 함수에 암묵적 입력과 출력 찾기
    -   암묵적 입력 : 함수를 부르는 동안 결과에 영향을 줄 수 있는 것
    -   암묵적 출력 : 함수 호출의 결과로 영향 받는 것
    -   함수 인자를 포함해 함수 밖에 있는 변수를 읽거나 데이터베이스에서 값을 가져오는 것 : 입력
    -   리턴값을 포함해 전역변수를 바꾸거나, 공유 객체를 바꾸거간, 웹 요청을 보내는 것 : 출력
3.  암묵적 입력은 인자로 바꾸고, 암묵적 출력은 리턴값으로 바꾸기
    -   **인자와 리턴값은 바뀌지 않는 불변값**이라는 것이 중요
    -   리턴값이 나중에 바뀐다 → 암묵적 출력
    -   인자로 받은 값이 바뀔 수 있다 → 암묵적 입력

# 📘 연습 문제

## ✅ update_tax_dom 함수에서 세금 계산 부분 추출하기

```tsx
update_tax_dom(){
	set_tax_dom(shopping_cart_total * 0.10);
}

```

암묵적 입력 : shopping_cart_total → 전역 변수 참조(읽기) ⇒ 인자로 받아 명시적 입력
암묵적 출력 : set_tax_dom() → DOM 조작 ⇒ 리턴값으로 명시적 출력

```tsx
update_tax_dom(total){
	const tax = total * 0.10;
	return tax;
}

```

### 🚨 **DOM 업데이트와 비즈니스 규칙 분리하기**

calc_tax 함수가 비즈니스 규칙 담당
set_tax_dom 함수가 DOM 업데이트 담당

```tsx
update_tax_dom(total){
	set_tax_dom(calc_tax(shopping_cart_total));
}

function calc_tax(amount){
	return amount * 0.10;
}

```

## ✅ update_shipping_icons() 함수에서 계산 추출하기

무료 배송인지 확인하는 코드 작성

```tsx
function update_shipping_icons(){
	const buy_buttons = get_buy_buttons_dom();
	for(let i = 0; i < buy_buttons.length; i++){
		const button = buy_buttons[i];
		const item = button.item;
		if(**item.price + shopping_cart_total >= 20**)
			button.show_free_shipping_icon();
		else
			button.hide_free_shipping_icon();
	}
}

```

비즈니스 로직 햠수로 분리하기
함수의 인자로 받아 암묵적 입력 제거하기

```tsx
function isFreeShipping(price, total){
	return price + total >= 20;
}

function update_shipping_icons(){
	const buy_buttons = get_buy_buttons_dom();
	for(let i = 0; i < buy_buttons.length; i++){
		const button = buy_buttons[i];
		const item = button.item;
	if (isFreeShipping(item.price, shopping_cart_total))
			button.show_free_shipping_icon();
		else
			button.hide_free_shipping_icon();

```
