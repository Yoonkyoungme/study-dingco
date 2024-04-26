# 12. 함수형 반복

### 함수형 도구 map()

```js
function emailsForCustomers(customers, goods, bests) {
  var emails = [];
  for (var i = 0; i < customers.length; i++) {
    var customer = customers[i];
    var email = emailForCustomer(customer, goods, bests);
    emails.push(email);
  }
  return emails;
}

function emailsForCustomers(customers, goods, bests) {
  return map(customers, function (customer) {
    return emailForCustomer(customer, goods, bests);
  });
}
```
