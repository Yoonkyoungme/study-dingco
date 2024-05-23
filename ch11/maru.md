# chapter 11. 일급 함수 2

>
**[ 이번 장에서 살펴볼 내용 ]**
- 함수 본문을 콜백으로 바꾸기 리팩터링에 대해 더 알아봅시다.
- 함수를 리턴하는 함수가 가진 강력한 힘을 이해합니다.
- 고차 함수에 익숙해지기 위해 여러 고차 함수를 만들어 봅니다.

# 카피-온-라이트 리팩터링하기

**리팩터링으로 얻은 것**

- 표준화된 원칙
- 새로운 동작에 원칙을 적용할 수 있음
- 여러 개를 변경할 때 최적화

1. 본문과 앞부분, 뒷부분 확인하기
- 복사하고 리턴하는 부분이 중복

```jsx
function arraySet(array, idx, value) {
	var copy = array.slice(); // 복사
	copy[idx] = value;
	return copy; // 리턴
}
```

2. 리팩터링 할 코드를 함수로 빼내기

```jsx
function arraySet(array, idx, value) {
	return withArrayCopy(array);
}

function withArrayCopy(array) {
	var copy = array.slice(); // 복사
	copy[idx] = value;
	return copy; // 리턴
}
```

3. 본문을 콜백 함수로 빼는 단계

```jsx
function arraySet(array, idx, value) {
	return withArrayCopy(array, function(copy) {
		copy[idx] = value;
		});
}

function withArrayCopy(array, modify) {
	var copy = array.slice(); // 복사
	modify(copy);
	return copy; // 리턴
}
```

# 함수를 리턴하는 함수

```jsx
try {
	saveUserData(user);
} catch(error) {
		logToSnapErrors(error);
}
```

로그를 남겨야 하는 모든 함수에 try/catch 구문을 감싸야함

```jsx
function withLogging(f) {
	try {
		f();
	} catch(error) {
		logToSnapErrors(error);
	}
	
	withLogging(function() { 
		saveUserData(user);
	});
```

코드를 감싸지 않고 그냥 함수를 호출할 수 있는 방법 → `고차함수`

```jsx
function wrapLogging(f) {
return function(arg) {
		try {
			f(arg);
		} catch(error) {
			logToSnapErrors(error);
		}
}

var saveUserDataWithLogging = wrapLogging(saveUserDataNoLogging);
```

### **고차 함수를 만들 때 생각해보기**

- 코드가 더 읽기 쉬운가요?
- 얼마나 많은 중복 코드를 없앨 수 있나요?
- 코드가 하는 일이 무엇인지 쉽게 알 수 있나요?
