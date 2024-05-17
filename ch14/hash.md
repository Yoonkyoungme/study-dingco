# 14. 중첩된 데이터에 함수형 도구 사용하기

함수 이름에 있는 암묵적 인자, 암묵적 인자를 드러내기

update() : 바꿀 객체, 키, 바꾸는 동작(함수) 넘기는
특정 값을 다루는 동작을 받아 특정키가 있는 해시 맵에 적용
=> 이게 무슨뜻이지..?

### 조회하고 변경하고 설정하는 것을 update 로 교체하기

1. 객체에서 값 조회
2. 값 바꾸기
3. 객체에 값을 설정

```js
function incrementField(item, field) {
  var value = item[field]; // 조회
  var newValue = value + 1; // 값 바꾸기
  var newItem = objectSet(item, field, newValue);
  // 객체에 값 설정
  return newItem;
}
//이 코드를 아래처럼

function incrementField(item, field) {
  return update(item, field, function (value) {
    return value + 1;
  });
}

function update(object, key, modify) {
  var value = object[field]; // 조회
  var newValue = modify(value); // 값 바꾸기
  var newObject = objectSet(object, key, newValue);
  // 객체에 값 설정
  return newObject;
}
```

update 는 중첩된 객체에 사용하면 좋음

즉, update 는 객체에서 객체, 키, 수정로직 넘겨주면 객체의 특정 키의 값을 수정로직에 따라 수정해줌

### 객체에 있는 값 시각화 하기

nestedUpdate

const update4(object,key1,key2,key3,key4,modify){}
이것은 4번 업데이트 함.
=> update3 을 불러주면 됨.
즉,
재귀함수로 표현가능

```js
updateX(object,depth,keys=[key1,key2,key3,key4],modify){
    if(keys.length ===0){
        return modify(object)
    }//종료조건
    return update(object,key1,function(value1){
        return updateX(value1,depth-1, key2, key3, key4, modify)
    })
}
```

이런식으로..!
=> 키, 개수, 순서가 중요해짐

for 문이 이해하기 더 쉽더라도, 데이터를 다루는 경우에는 재귀로 만드는 것이 더 명확!!

### 안전한 재귀 사용법

- 종료 조건 설정
  찾아야 할 것이 없을 때, 배열 인자가 비어쓸 떄...

- 재귀 호출
  재귀 함수는 최소 하나의 재귀 호출이 있어야 함!
- 종료 조건에 다가가기
  재귀함수는 최소 하나 이상의 인자가 점점 줄어들어야 함.
  가장 좋지 않은 것은 재귀 호출에 같은 인자를 그대로 전달하는 것!

중첩 업데이트

```json
{
  "cart": {
    "shirt": {
      "options": {
        "color": "blue",
        "size": 3
      }
    }
  }
}
```

여기서 color 의 값을 바꾸기 위해 update3
, nestedUpdate 사용.

## 깊이 중첩된 구조를 설계할 때 생각할 점

nestedUpdate 를 쓰려면 긴 키 경로가 필요함.
=> 추상화 벽 사용해서 해결하기!!!
function updatePostId(~~~){
return nestedUpdate(category,['posts','id'],modify)

}
이런식으로 긴 key 값을 기억하기 쉽게 함수 이름에 명시하기

->['posts','id'] 와 같은 분류의 구조 같은 구체적인 부분은 추상화 벽 뒤로 숨김
