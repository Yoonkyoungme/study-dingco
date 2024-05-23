>
[ 이번 장에서 살펴볼 내용 ]
- `해시 맵에 저장된 값을 다루기 위한 고차 함수`를 만듭니다.
- 중첩된 데이터를 고차 함수로 쉽게 다루는 방법을 배웁니다.
- 재귀를 이해하고 안전하게 재귀를 사용하는 방법을 살펴봅니다.
- 깊이 중첩된 엔티티에 `추상화 벽`을 적용해서 얻을 수 있는 장점을 이해합니다.

# 📘 update() 도출하기
- 명시적으로 바꿔야 할 인자가 일반값이 아니고 `동작`
    - 함수 본문을 콜백으로 바꾸기 리팩터링으로 **동작을 함수 인자로 받도록 한다**
- update : 객체에 있는 값을 바꾸고 `카피-온-라이트 원칙` 에 따라 새로운 객체를 만들어 반환

<aside>
💡 조회하고 변경하고 설정하는 것을 update로 교체하기

</aside>

```tsx
function update(object, key, modify) {
	const value = object[key];
	const newValue = modify[value];
	const newObject = objectSet(object, key, newValue);
	return newObject;
}
```

- update() 에서 리턴한 값을 원래 변수에 재할당해서 사용
- **계산(월급 계산)과 액션(상태 변경)이 분리**된 모습

```tsx
const employee = {
	name : 'Kim',
	salary: 120000
}

employee = update(employee, salary, raise10Percent);
```

# 📘 중첩된 update 시각화하기

```tsx
// before
function incrementSize(item) {
	const options = item.options;
	const size = options.size;
	const newSize = size + 1;
	const newOptions = objectSet(options, 'size', newSize);
	const newItem = objectSet(item, 'options', newOptions);
	return newItem;
}

// 1번 리팩터링
function incrementSize(item) {
	const options = item.options;
	const newOptions = update(options, 'size', increment);
	
	const newItem = objectSet(item, 'options', newOptions);
	return newItem;
}

// 2번 리팩터링
function incrementSize(item) {
	return update(item, 'options', function(options) {
		return update(options, 'size', increment);
	});
}
```

<aside>
💡 데이터가 중첩된 단계만큼 update를 호출해야 한다

</aside>

# 📘 nestedUpdate() 도출하기

- 중첩된 개수에 상관없이 쓸 수 있는 nestedUpdate() 만들기
- 깊이와 키 개수를 어떻게 맞출 수 있을까?
    - 키의 개수와 순서가 중요 → `배열 사용`

```tsx
function updateX(object, keys, modify) {
	if (keys.length === 0)
		return modify[object];
		
	const key1 = keys[0];
	const restOfKeys = drop_first[keys];
	return update(object, key1, function(value1) {
		return updateX(value1, restOfKeys, modify);
	});
}
```

**재귀 함수가 적합한 이유**

- 점점 아래 단계로 내려가면서 최종값에 도착하면 값을 변경하고 나오면서 새로운 값을 설정
    - **가장 아래 단계에 도착하면 값을 변경**

**주의할 점**

- 중간 객체들은 서로 다른 키를 가지고 있지만 nestedUpdate() 경로를 보고 **어떤 키가 있을지 알 수 없음**

# 📘 중첩 객체에 추상화 벽 사용하기

- 중첩된 각 단계의 데이터 구조를 모두 기억해야 하는 어려움 존재
- 같은 작업을 하면서 알아야 할 데이터 구조를 줄이는 것 → `추상화 벽`

**추상화 벽에 함수를 만들고 의미 있는 이름을 붙여주는 것**

```tsx
function updatePostById(category, id, modifyPost) {
	return nestedUpdate(category, ['posts], id], modifyPost);
}

function updateAuthor(post, modifyUser) {
	return update(post, 'author' ,modifyUser);
}

function capitalizeName(user) {
	return update(user, 'name', capitalize);
}

updatePostById(blogCategory, '12', function(post) {
	return updateAuthor(post, capitalizeUserName);
}
```
