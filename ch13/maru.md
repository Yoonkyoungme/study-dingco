>
[ 이번 장에서 살펴볼 내용 ]
- 복합적인 쿼리로 데이터를 조회하기 위해 함수형 도구를 조합하는 방법을 배웁니다.
- 복잡한 반복문을 함수형 도구 체인으로 바꾸는 방법을 이해합니다.
- 데이터 변환 파이프라인을 만들어 작업을 수행하는 방법을 배웁니다.

---

- `체이닝` : 여러 단계를 하나로 조합하는 것
- 코드에 중첩된 리턴 구문이 있는 콜백이 있을 경우, 코드가 어떤 일을 하는지 알기 어려움

## 체인을 명확하게 만들기

### 1. 단계에 이름 붙이기

```tsx
// 단계에 이름 붙이기 (before)
function biggestPurchaseBestCustomers(customers) {
	const bestCustomers = filter(customers, function(Customer) {
		return customer.purchases.length >= 3;
	}
	...
}

// 단계에 이름 붙이기 (after)
function biggestPurchaseBestCustomers(customers) {
	const bestCustomers = selectBestCustomers(customers);
	...
}

function selectBestCustomers(customers) {
	return filter(customers, function(Customer) {
		return customer.purchases.length >= 3;
	}
}
```

### 2. 콜백에 이름 붙이기

```tsx
// 콜백에 이름 붙이기 (before)
function biggestPurchaseBestCustomers(customers) {
	const bestCustomers = filter(customers, function(Customer) {
		return customer.purchases.length >= 3;
	}
	...
}

// 콜백에 이름 붙이기 (after)
function biggestPurchaseBestCustomers(customers) {
	const bestCustomers = selectBestCustomers(customers, isGoodCustomer);
	...
}

function isGoodCustomer(customer) {
	return customer.purchases.length >= 3;
}
```

### 3. 두 방법을 비교

- 일반적으로 2번째 방법이 더 명확.
- 콜백에 이름을 붙이는 방법이 재사용하기 더 좋음
- 인라인 대신 이름을 붙여 콜백을 사용하면 단계가 중첩되는 것도 막을 수 있음

### 배열 생성

- filter 또는 map 을 사용하면 새로운 배열을 만드는데, 함수가 호출될 때마다 새로운 배열이 생기기 때문에 비효율적이라고 생각할 수 있음
- But, 만들어진 배열이 필요없을 때 `가비지 컬렉터` 가 빠르게 처리하여 문제 없음
- 비효율적인 경우 최적화하기 쉬움
- 스트림 결합(stream fusion) : 자바스크립트 메서드 체인을 최적화하는 것
- 최적화 과정은 병목이 생겼을 때만 쓰는 것이 좋고, 대부분 여러 단계를 사용하는 것이 더 명확하고 읽기 쉬움

### 반복문을 함수형 도구로 리팩터링하기

1. 데이터 만들기
2. 한 번에 전체 배열을 조작하기
3. 작은 단계로 나누기

**1. 데이터 만들기**

```tsx
const answer = [];
const window = 5;

for(let i = 0; i < array.length; i++) {
	let sum = 0;
	let count = 0;
	for(let i = 0; i < window; w++) {
		let idx = i + w;
		if (idx < array.length) {
			sum += array[idx];
			count += 1;
		}
	}
	answer.push(sum/count);
}
```

- 특정 범위의 값을 새로운 배열로 만들어 반복하면 어떻게 될까요?

```tsx
const answer = [];
const window = 5;

for(let i = 0; i < array.length; i++) {
	let sum = 0;
	let count = 0;
	**const subarray = array.slice(i, i + window);**
	for(let w = 0; w < subarray.length; w++) {
			sum += subarray[w];
			count += 1;
		}
	}
	answer.push(sum/count);
}
```

1. **한 번에 전체 배열을 조작하기**

```tsx
const answer = [];
const window = 5;

for(let i = 0; i < array.length; i++) {
	const subarray = array.slice(i, i + window);
	for(let w = 0; w < subarray.length; w++) {
	**answer.push(average(subarray));**
}
```

1. **작은 단계로 나누기**

```tsx
const answer = [];
const window = 5;

const indices = range(0, array.length);
const windows = map(indices, function(i) {
	return array.slice(i, i + window);
});
const answer = map(windows, average);

function range(start, endO {
	const ret = [];
	for(let i = start; i < end; i++){
		ret.push(i);
	}
}
```

# 값을 만들기 위한 reduce()

- reduce()는 값을 요약하는 역할과 `값을 만드는 역할` 존재
- 고객이 장바구니를 잃어버렸지만 운좋게 고객이 장바구니에 추가한 제품을 모두 배열로 로깅하고 있었음
- 로깅 배열 데이터로 현재 장바구니 상태 만들기 → `reduce()`
- 데이터가 삭제되는 경우도 모두 처리해준다면 `[’add’, ‘shirt’]` 와 같은 방식으로 받아서 어떤 동작인지도 데이터로 표현할 수 있다.

```tsx
const cartLogging = ['shirt', 'shoes', 'shirt' ...];

const shoppingCart = reduce(cartLogging,  {}, function(cart, item) {
	if(!cart[item]){ // 해당 물건을 처음 담는 경우
		return add_item(cart, {name: item, quantity: 1, price: priceLoopup(item)};
	} else { // 해당 물건이 장바구니에 이미 있는 경우
		const quantity = cart[item].quantity;
		return setFieldByName(cart, item, 'quantity', quantity + 1);
	}
});
```
