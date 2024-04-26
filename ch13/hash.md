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

항등함수 : 인자로 받은 값을 그대로 리턴하는 함수. 아무 일도 하지 않아야 할 때 유용하게 쓰일 수 있다.
ex)

```js
function max(array, init) {
  return maxKey(array, init, function (x) {
    return x;
  });
}
//값을 직접 비교해야함.
// max(배열, )어떻게 사용하징...
```
