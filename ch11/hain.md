# 11. 일급함수 2

## 📝 핵심 내용

- 코드의 중복을 없애고 더 좋은 추상화를 하기 위한 방법

  - 1. 암묵적 인자를 드러내기 : 함수 이름에 있는 암묵적 인자를 명시적 인자로 바꾸기
  - 2. 함수 본문을 콜백으로 바꾸기 : 공통적인 부분을 함수로 만들고, 바뀌는 부분을 콜백으로 넘기기

    - 1. 콜백으로 함수 본문 넘겨주는 방법 => `withLogging`으로 감싸는 부분 계속 반복됨!

    ```js
    function withLogging(f) {
      try {
        f();
      } catch (error) {
        logToSnapErrors(error);
      }
    }

    withLogging(() => {
      saveUserData(user);
    });
    ```

    - 2. 코드를 감싸지 않고 호출할 수 있도록 만드는 방법(함수를 리턴하는 함수 => 함수 팩토리!)

    ```js
    function wrapLogging(f) {
      return function (...arg) {
        try {
          f(...arg);
        } catch (error) {
          logToSnapErrors(error);
        }
      };
    }

    const saveUserDataWithLogging = wrapLogging(saveUserData);
    ```

## ❣️ 느낀 점

- 함수형 프로그래밍을 잘 하기 위해서는 고차함수를 잘 활용할 줄 알아야 한다는 생각이 들었다.
- (287p.) "어떤 방법이 더 좋을까요? 코드가 더 읽기 쉬운가요? 얼마나 많은 중복 코드를 없앨 수 있나요? 코드가 하는 일이 무엇인지 쉽게 알 수 있나요? 이런 질문들을 놓치면 안됩니다. 고차함수는 강력한 기능입니다. 하지만 비용이 따릅니다. 만드는 재미에 빠져 읽을 때 문제를 보지 못하면 안 됩니다. 능숙하게 쓸 줄 알아야 하지만 더 좋은 코드를 만드는 데 써야 합니다." 우리가 중복을 줄이기 위해 고차함수를 쓰는 이유도, 함수형 프로그래밍을 배우는 이유도 모두 '좋은 코드'를 만들기 위해서라는 대전제를 잊지 말자..!

## 🤔 궁금한 점 & 더 생각해볼 점

- 위에서 try/catch문을 자동으로 감싸주는 함수를 2개 만들었다. 함수로 감싸는 것이 더 가독성이 좋을까, 아니면 함수 팩토리를 통해 함수를 반환받아 사용하는 것이 더 가독성이 좋을까?

  - 👉 나의 생각: 개인적으로 인자가 있는 함수를 써야하는 경우에는 함수 팩토리를 이용하는 것이 더 좋다고 생각한다. 왜냐하면 함수에 인자를 넣어주기 위해 `() => {fn(인자)}` 이렇게 넘기는 것이 가독성이 좋지 않다고 느끼기 때문이다. 그러나 만약 인자를 넘겨줄 필요 없다면 `withLogging(saveDate)` 처럼 함수로 감싸는 것이 더 가독성이 좋아보이는 것 같다.

  ```js
  // 인자가 없는 경우 사용하는 부분 비교
  // 1
  withLogging(saveUserData);

  // 2
  const saveUserDataWithLogging = wrapLogging(saveUserData);
  saveUserDataWithLogging();
  ```

  ```js
  // 인자가 있는 경우 사용하는 부분 비교

  // 1
  withLogging(() => saveUserData(user));

  // 2
  const saveUserDataWithLogging = wrapLogging(saveUserData);
  saveUserDataWithLogging(user);
  ```

## 🚀 적용해본 점
