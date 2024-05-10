## update() 도출하기

- 키로 대상 객체의 값을 조회, 입력 받은 함수로 값을 변경하여 리턴한다.

```javascript
function update(object, key, modify) {
  // 객체, 바꿀 값의 위치(키), 바꾸는 동작을 인자로 받음.
  var value = object[key]; // 조회
  var newValue = modify(value); // 바꾸기
  var newObject = objectSet(object, key, newValue); // 설정
  return newObject;
}
```

<br />
<br />

## nestedUpdate() 도출하기

- `재귀 함수`를 활용하여 여러 번 중첩된 해시 맵에도 적용할 수 있는 update()를 구현한다.

```javascript
function nestedUpdate(object, keys, modify) {
  if (keys.length === 0) return modify(object);
  var key1 = keys[0];
  var restOfKeys = drop_first(keys);
  return update(object, key1, function (value1) {
    return nestedUpdate(value1, restOfKeys, modify);
  });
}
```

- 안전한 재귀 사용 (재귀 함수를 잘못 사용하여 무한 반복에 빠지지 않게 하기)
  - 종료 조건(base case): 재귀를 멈추기 위한 조건을 명시한다. (배열 인자가 비었거나, 점점 값이 줄어들어 0이 된다.)
  - 재귀 호출(recursive call): 재귀 함수는 최소 하나의 재귀 호출이 있어야 한다.
  - 종료 조건에 다가가기: 각 재귀 호출에서 최소 하나 이상의 인자가 줄어들어 한 단계씩 종료 조건에 가까워져야 한다.
