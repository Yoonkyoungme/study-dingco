# chapter 10. 일급 객체 1

>
**[ 이번 장에서 살펴볼 내용 ]**
- 왜 일급 값이 좋은지 알아봅시다.
- 문법
- 고차 함수로 문법을 감싸는 방법을 알아봅시다.
- 일급 함수와 고차 함수를 사용한 리팩터링 두 개를 살펴봅시다.

일급 함수가 무엇인가? 어디에 사용되는가?

일급 함수는 어떻게 만드는가?

- code smell : 함수 이름에 있는 암묵적 인자
    - 함수 구현이 거의 똑같다.
    - 함수 이름이 구현의 차이를 만든다.
- 암묵적 인자 드러내기
    - 명시적인 인자 추가
    - 함수 본문에 하드 코딩된 값을 새로운 인자로 바꾸기
- 함수 본문을 콜백으로 바꾸기
    - 함수 본문에 바꿀 부분의 앞부분과 뒷부분 확인
    - 리팩터링 할 코드를 함수로 빼내기
    - 빼낸 함수의 인자로 넘길 부분을 또 다른 함수로 빼내기

# 암묵적 인자 드러내기

필드를 결정하는 문자열이 함수 이름에 있다 → 암묵적 인자 드러내기
리팩터링으로  비슷한 함수를 **일반적인 함수 하나로 수정**

```jsx
// ❌
function setPriceByName(cart, name, price) {
	var item = cart[name];
	var newItem = objectSet(item, 'price', price);
	var newCart = objectSet(cart, name, newItem);
	return newCart;
}

// ✅
function setFieldByName(cart, name, field, value) {
	var item = cart[name];
	var newItem = objectSet(item, field, value);
	var newCart = objectSet(cart, name, newItem);
	return newCart;
}

cart = setFieldByName(cart, name, 'price', 10);
```

`일급 값` : 함수의 인자로 넘길 수 있고, 함수의 리턴값으로 받을 수 있고, 변수에 할당할 수 있는 값

**일급이 아닌 것을 일급으로 바꾸는 방법**을 아는 것이 중요

**필드명을 문자열로 사용할 때 실수를 방지**하는 방법

1. 컴파일 타임 검사
    - 정적 타입 시스템에서 사용하는 방법
    - 사용할 수 있는 문자열 필드인지 확인 가능
2. 런타임 검사
    - 함수를 실행할 때마다 동작
    - 전달한 문자열이 올바른 문자열인지 확인

엔티티 필드명을 일급으로 만들어 세부 구현이 밖으로 노출된 것이 아닌가? → ❌

추상화 벽 위(외부)에서 quantity 필드명을 그대로 사용하고 싶다면 내부에서 필드명을 바꾸면 된다

→ 새로운 필드명으로 바꿔주는 객체 사용

데이터를 사용할 때 임의의 인터페이스로 감싸지 않고 그대로 사용하고 있다는 점

인터페이스를 잘 만들면 데이터를 정해진 방법으로만 쓸 수 있음

**But, 일반적인 엔티티는 객체와 배열처럼 일반적인 데이터 구조를 사용해야함**

데이터가 어떤 방법으로 해석될지 미리 알 수 없으므로 필요할 때 적절히 해석할 수 있어야 함 

→ `데이터 지향` : 이벤트와 엔티티에 대한 사실을 표현하기 위해 일반 데이터 구조를 사용하는 형식

## 고차함수 만들기 (HOC)

**코드가 비슷하지만 하는 일이 다를 때** 문법적으로 비슷한 부분을 찾아 하나의 함수로 만들기

```jsx
function cookAndEatFoods() {
	for (var i = 0; i < foods.length; i++) {
		var item = foods[i];
		cook(item);
		eat(item);
	}
}
```

1. 함수 이름에 있는 암묵적 인자 확인 (Foods → Array)
2. 명시적인 인자 추가 (array)

```jsx
function cookAndEatArray(array) {
	for (var i = 0; i < array.length; i++) {
		var item = array[i];
		cookAndEat(item);
	}
}

function cookAndEat(food) {
	cook(food);
	eat(food);
}
```

1. 함수 본문에 하드 코딩된 값을 새로운 인자로 교체
2. 함수 호출하는 곳 수정

```jsx
function operateOnArray(array, fn) {
	for (var i = 0; i < array.length; i++) {
		var item = array[i];
		fn(item);
	}
}

function cookAndEat(food) {
	cook(food);
	eat(food);
}

**operateOnArray(foods, cookAndEat);**
```

고차 함수의 좋은 점 → **코드를 추상화할 수 있음**

## 함수 본문을 콜백으로 바꾸기

1. 코드에서 본문 앞뒤로 바뀌지 않는 부분을 찾는다

```jsx
try {
	saveUserData(user);
} catch(error) {
	logToSnapErrors(error);
}
```

2. 전체를 함수로 빼낸다.

```jsx
function withLogging() {
	try {
		saveUserData(user);
	} catch(error) {
		logToSnapErrors(error);
	}
}

withLogging();
```

3. 앞뒤 사이에 있는 본문을 인자로 빼낸다.

```jsx
function withLogging(fn) {
	try {
		fn();
	} catch(error) {
		logToSnapErrors(error);
	}
}

withLogging(() => saveUserData(user));
```

### **본문을 함수로 감싸서 넘기는 이유**

- 바로 실행되면 안되기 때문(함수의 실행을 미루는 방법)

- 함수 안에 있는 코드가 **특정한 문맥 안에서 실행되어야 한다**

- → `고차 함수` 를 쓰면 다른 곳에 정의된 문맥에서 코드를 실행할 수 있다
