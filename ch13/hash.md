# 13. 함수형 도구 체이닝

체이닝 : 여러 단계를 하나로 조합하는 것.

```js
maxKey(customer.purchases, { total: 0 }, function (purchase) {
return purchase.total;
});

function maxKey(array,init,f){
  return reduce(array,init,function(biggestSoFar,element {
    if(f(biggestSoFar)>f(element)){
  return biggestSoFar;
}
else return element
}))
}


function biggestPurchasesBestCustomers{
...

    var biggestPurchases = map(bestCustomers,function(customer){
        return maxKey(customer.purchases,{total:0},function(purchase){return purchase.total})
    })

    // maxKey 의 인자로 구매가격, 총숫자, 함수를 전달해줌.
    // maxKey 는 이 인자를 가지고, 만약에 전달해준 함수의 결과값이 엘리먼트보다 크면 biggestSoFar 을, 작으면 엘리먼트를 반환해준다.
    // 여기서는 전달해준 함수가 구매가격의 total 값 이므로 biggestSoFar.total 과 element.total 을 비교한다.

    // 즉 배열에서 가장 큰 값을 찾는 함수인데, 이제 객체에 접근하는 함수를 인자로 받아서 유연하게 대응할 수 있도록 했다.

}

```

항등함수 : 인자로 받은 값을 그대로 리턴하는 함수. 아무 일도 하지 않아야 할 때 유용하게 쓰일 수 있다. ex)

```js
function max(array, init) {
  return maxKey(array, init, function (x) {
    return x;
  });
}
//값을 직접 비교해야함.
// max(배열, )어떻게 사용하징...
```

## 체인을 명확하게 만들기

### 1. 단계에 이름 붙이기

고차함수를 빼내 이름 붙이기

```js
ex) var bestCustomers = selectBestCustomer()

function selectBestCustomer(customer){
  return filter(customer,function (customer)=>{
    return customer.purchases.length >= 3
})
}

```

### 2. 콜백에 이름 붙이기

```js
ex) var bestCustomers = filter(customers, isGoodCustomer)

function isGoodCustomer(customer){
  return customer.purchases.length >= 3
}
```

filter, map 은 새 배열 호출될때마다 새 배열 생성..
=> 그러나 만들어진 배열이 필요 없을 떄 가비지 컬렉터가 빠르게 처리하기 때문에 괜찮음. 특히 스트림 결합(쉽게 최적화 되어서 다시 반복문으로 돌아가지 않아도 됨.) 최적화를 위해 map, reduce 등을 같이 사용할 수 있음(단 따로 적는게 더 가독성 있음. 최적화를 위함임)

### 반복문을 함수형 도구로 리팩터링

1. 이해하고 다시 만들기
2. 단서를 찾아 리팩터링
   안쪽 반복문은 리팩터링 시작하기 좋은 위치!

리펙터링 팁 :

- 데이터 만들기
  값을 배열에 넣어서 반복할 수 있게 만들기

```js
//바깥 for : array 만큼 반복
for (var w = 0; w < window; w++) {
  //안쪽 for : 0~window 까지 반복
  var idx = i + w;
  if(idx < array.length){
    ~~~~
  }
  // for 내부를 보면 0~window-1 까지 변하지만 배열로 만들지 않음!!

}

-> 내부 값 배열로 만들어주기
// 배열을 window 까지 잘라서 돌려주기,
var subarray = array.slice(i,i+window)
이 배열 돌려주기
for(var w=; w<subarray.length;w++){
  sum += subarray[w];
  count +=1;
}
```

- 배열 전체를 다루기
  위에서 하위 배열을 만들어서 배열 전체를 반복할 수 있음!
  반복문을 대신에 전체 배열을 한번에 처리할 수 있는지 생각해보자,

- 작은 단계로 나누기
  배열의 항목이 아니라 인덱스를 가지고 반복해야 하는 문제가 있음!!

```js
var answer = map(indices, function(i){
  각 항목마다 인덱스 가지고 콜백 부름
})
```

재사용 하기 좋은지, 테스트 하기 쉬운지, 유지보수 하기 쉬운지에 따라 호출 그래프에서의 위치가 정해ㅈ리 수 있음!!!

- 조건문을 filter 로 바꾸기

- 유용한 함수로 추출하기
- 개선을 위해 실험하기

## 체이닝 디버깅을 위한 팁

- 구체적인 것을 유지하기
  기억하기 쉽게 네이밍 붙이기!
- 출력해보기
  중간중간 console.log 를 통해 예상대로 동작하는지 확인!!
- 타입 따라가보기
  콜백이 리턴하는 타입의 값이 reduce , map 등의 결과값!!

## 다양한 함수형 도구

- pluck()

- concat()

- frequenciesBy()
- groupBy()

## 값을 만들기 위한 reduce()

## 데이터를 사용해 창의적으로 만들기

->인자를 데이터로 만들면 함수형 도구 체이닝하기 좋음!!!!
