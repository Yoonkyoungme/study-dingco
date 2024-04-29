# 12. 함수형 반복

## 📝 핵심 내용

- 배열에 대한 반복문을 함수형 도구로 바꾸기
  - map : 배열의 모든 항목에 함수를 적용한 새로운 배열로 만든다.
  - filter: 배열의 하위 집단을 선택해 새로운 배열로 만든다.
  - reduce: 초기값을 기준으로 어떤 배열의 항목들을 조합해 하나의 값을 만든다.

## ❣️ 느낀 점

- 평소 map과 filter는 많이 사용했으나 reduce는 잘 사용하지 않았었다. 그런데 모든 항목을 더하는 것 이외에도, 실행 취소 및 복귀, 사용자 입력을 순서대로 재현하기, 시간 여행 디버깅 등 다양하게 사용될 수 있다는 것이 인상깊었다.

## 🤔 궁금한 점 & 더 생각해볼 점

- map, filter, reduce 이외에 또 우리가 유용하게 쓸 수 있는 반복을 위한 함수형 도구가 무엇이 있을까?
  - every() : 배열의 각 요소에 대해 제공된 callbackFn 함수를 한 번씩 호출하고, callbackFn이 거짓 값을 반환할 때까지 호출을 반복. 거짓 요소가 발견되면 every()는 즉시 false를 반환하고 배열 순회를 중지
  - some() : 배열 안의 어떤 요소라도 주어진 판별 함수를 적어도 하나라도 통과하면 true를 반환.(true를 반환하며 배열 순회를 멈춤) 그렇지 않으면 false를 반환.

## 🚀 적용해본 점

```js
const cardNumbersValue = cardNumberInputs.map((input) => input.value);
```

```js
function map(arr, callback) {
  const result = [];

  for (let i = 0; i < arr.length; i++) {
    result.push(callback(arr[i]));
  }

  return result;
}
```

```js
function filter(arr, callback) {
  const result = [];

  for (let i = 0; i < arr.length; i++) {
    if (callback(arr[i])) {
      result.push(arr[i]);
    }
  }

  return result;
}
```

```js
function reduce(arr, callback, initValue) {
  let result = initValue;

  for (let i = 0; i < arr.length; i++) {
    result = callback(result, arr[i]);
  }

  return result;
}
```
