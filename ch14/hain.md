# 14. 중첩된 데이터에 함수형 도구 사용하기

## 📝 핵심 내용

- 중첩된 데이터는 재귀를 사용하여 다룰 수 있다.

```js
function update(object, key, modify) {
  const value = object[key];
  const newValue = modify(value);
  const newObject = objectSet(object, key, newValue);

  return newObject;
}

function updateX(object, keys, modify) {
  if (keys.length === 0) {
    return modify(object);
  }
  const key = keys[0];
  const restOfKeys = dropFirst(keys);

  return update(object, key, function (value) {
    return updateX(value, restOfKeys, modify);
  });
}
```

- 많은 키를 가지고 있는 중첩된 구조의 경우, 추상화의 벽으로 쉽게 다룰 수 있다.

## ❣️ 느낀 점

- 배열을 다루는 고차함수는 일상적으로 많이 사용해보았으나, 객체나 중첩된 구조를 다루는 고차 함수는 많이 다뤄보지 못한 것 같다. 우리 서비스에서 사용하는 데이터를 잘 다룰 수 있는 다양한 함수형 도구에 대해 미리 추상화를 해두는 것이 중요하다는 생각이 들었다.

## 🤔 궁금한 점 & 더 생각해볼 점

- update 이외에도, 객체를 다루는 고차함수를 만들어보자!

```js
const mapForObjValues = (object, fn) => {
  const result = {};
  for (const key in object) {
    result[key] = fn(object[key]);
  }

  return result;
};

const filterForObjValues = (object, fn) => {
  const result = {};
  for (const key in object) {
    if (fn(object[key])) {
      result[key] = object[key];
    }
  }

  return result;
};

const reduceForObjValues = (object, fn, initValue) => {
  let result = initValue;
  for (const key in object) {
    result = fn(result, object[key]);
  }

  return result;
};
```

## 🚀 적용해본 점
