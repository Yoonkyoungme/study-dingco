## 코드의 냄새: 함수 이름에 있는 암묵적 인자

- 함수 본문에서 사용하는 어떤 값이 함수 이름에 나타난다면 `함수 이름에 있는 암묵적 인자`는 코드의 냄새가 된다.
  - 코드의 냄새: 인자로 필드를 넘기는 대신 함수 이름의 일부분이 되어 있는 것
- 함수 이름에 있는 암묵적 인자 냄새는 두 가지 특징을 가진다.
  - 함수 구현이 거의 똑같다.
  - 함수 이름이 구현의 차이를 만든다.

<br />
<br />

## 리팩터링: 암묵적 인자를 드러내기

- 함수 이름에 있는 암묵적 인자를 어떻게 명시적인 함수 인자로 바꿀 수 있을까?

  - 암묵적 인자를 드러내기 리팩터링은 암묵적 인자가 일급 값이 되도록 함수에 인자를 추가한다. → 잠재적 중복을 없애고, 코드의 목적을 더 잘 표현할 수 있다.
  - 일급 값은 변수에 저장할 수 있고 인자로 전달하거나 함수의 리턴값으로 사용할 수 있다.

<br />
<br />

## 리팩터링: 함수 본문을 콜백으로 바꾸기

- 코드에서 본문 앞뒤로 바뀌지 않는 부분을 찾고, 본문 부분을 함수의 인자로 전달한다. → 고차 함수로 만들기
- 고차 함수: 다른 함수에 인자로 넘기거나 리턴 값으로 함수를 리턴할 수 있다.