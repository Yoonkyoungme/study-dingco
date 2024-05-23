```
💡 [ 이번 장에서 살펴볼 내용 ]
데이터가 바뀌지 않도록 하기 위해 카피-온-라이트를 적용합니다.
배열과 객체를 데이터에 쓸 수 있는 카피 -온-라이트 동작을 만듭니다.
깊이 중첩된 데이터도 카피-온-라이트가 잘 동작하게 만듭니다.
```

**불변성**

중첩된 데이터에 대해 제품의 정보를 바꿔야하는데, 불변형으로 만들 수 있는가?

읽기 또는 쓰기 또는 둘 다

읽기 : 데이터를 바꾸지 않고 정보를 꺼내는 것

인자에만 의존해 정보를 가져오는 읽기 동작→ 계산

쓰기 : 바뀌는 값은 어디서 사용될지 모르기 때문에 바뀌지 않도록 원칙이 필요

쓰기 동작은 불변성 원칙에 따라 구현 → 카피-온-라이트

데이터를 바꾸면서 동시에 정보를 가져오는 경우 → 가능

# 카피-온-라이트 원칙 3단계

```
💡 1. 복사본 만들기
2. 복사본 변경하기
3. 복사본 리턴하기
```

```jsx
function add_element_last(array, elem) {
  var new_array = array.slice(); // 복사본 만들기
  new_array.push(elem); // 복사본 바꾸기
  return new_array; // 복사본 리턴하기
}
```

## 카피-온-라이트로 쓰기를 읽기로 바꾸기

```jsx
// ❌ before
function remove_item_by_name(cart, name) {
	var idx = null;
		for(var i = 0; i < cart.length; i++) {
			if(cart[i].name === name)
				idx = i;
		}
		if (idx !== null)
			cart.splice(idx, 1);
}

// ✅ after
function remove_item_by_name(cart, name) {
	var new_cart = cart.slice(0;
	var idx = null;
		for(var i = 0; i < new_cart.length; i++) {
			if(new_cart[i].name === name)
				idx = i;
		}
		if (idx !== null)
			new_cart.splice(idx, 1);
		return new_cart;
}
```

## 쓰기 동작을 카피-온-라이트로 바꾸기

```jsx
function drop_first(array) {
  var array_copy = array.slice();
  array_copy.shift();
  return array_copy;
}
```

### 바뀔 때마다 복사하면 비효율적인 것 아닌가요?

불변 데이터 구조는 변경 가능한 데이터 구조보다 메모리를 더 많이 쓰고 느림

불변 데이터 구조를 사용하며 대용량의 고성능 시스템을 구현하는 사례 다수 존재

- 언제든 최적화할 수 있음
  - 미리 최적화하지 말고, 속도가 느린 부분이 있다면 그때 최적화하기
- 가비지 콜렉터는 매우 빠름
  - 제품이 100개인 배열을 복사해도 참조만 복사
  - 얕은 복사는 같은 메모리를 가리키는 참조에 대한 복사본을 만듦 → 구조적 공유
- 함수형 프로그래밍 언어에는 빠른 구현체 존재
  - 데이터 구조를 복사할 때 최대한 많은 구조를 공유
  - 더 적은 메모리를 사용하고, 가비지 콜렉터의 부담을 줄여줌

배열에 대한 카피-온-라이트 : `array.slice()`

객체에 대한 카피-온-라이트 : `Object.assign({}, object)`
