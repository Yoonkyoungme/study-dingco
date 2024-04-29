# 방어적 복사

원본이 바뀌지 않도록 막아줌

장바구니 값을 넘기기 전에 깊은 복사를 해서 함수에 넘김 (인자로 넘긴 원본이 바뀌지 않음)

함수 실행 후 결과값 → cart_copy

cart_copy를 안전하게 사용하기 위해 반환값도 복사

```jsx
var cart_copy = deepCopy(shopping_cart);
black_friday_promotion(cart_copy);
shopping_cart = deepCopy(cart_copy);
```

불변성을 스스로 구현할 수 있기 때문에 더 강력하지만 더 많은 데이터를 복사하므로 비용이 많이 듦

카피-온-라이트와 다르게 방어적 복사는 불변성 원칙을 구현하지 않은 코드로부터 데이터를 보호
