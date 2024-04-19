# 06. 변경 가능한 데이터 구조를 가진 언어에서 불변성 유지하기

## 📝 핵심 내용

- 데이터 읽기는 계산, 쓰기는 액션이었으나, 쓰기를 액션으로 바꿔주는 것이 바로 `카피-온-라이트`이다.
- 데이터의 불변성을 지키기 위해 `카피-온-라이트`를 사용해야 한다.
- 배열과 객체의 `카피-온-라이트`를 위해 유틸을 만들어 두는 것이 좋다.
- `카피-온-라이트`의 원칙 3단계
  - 1. 복사본을 만든다.
  - 2. 복사본을 변경한다.
  - 3. 복사본을 리턴한다.

## ❣️ 느낀 점

- 불변성을 유지하기 위해서 복사를 사용한다는 것은 알고있었는데, `카피-온-라이트`라는 보다 확실한 기법에 대해 알게 되어서 좋다.
- `카피-온-라이트`는 모든 팀원들이 함께 규칙을 지켜야 의미가 있을 것 같다는 생각을 했다.

## 🤔 궁금한 점

- 어차피 상태를 바꾸는건데 반드시 복사해서 바꿔야하는 이유가 있을까? 원본배열을 훼손하면 안된다고는 하지만 만약, class 상태를 공유하고 있다면, 복사한 값으로 상태를 변경해주면 다른 쪽은 어차피 바뀐 상태를 참조하게 된다. 어차피 변경할 값인데, 안전하게 변경할 수 있다면 굳이 복사를 해줘야 할까?

- 상황 1

  ```js
  const todoList = [];

  const addTodoList1 = (todoList, todo) => {
    todoList.push(todo);
  };

  const addTodoList2 = (todoList, todo) => {
    const copyTodoList = [...todoList];
    copyTodoList.push(todo);
    return copyTodoList;
  };
  ```

- 상황 2

  ```js
  class TodoList {
    private todoList

    constructor(){
        this.todoList = []
    }

    addTodoList1 (todoList, todo) {
    todoList.push(todo);
  };

  const addTodoList2 (todoList, todo) {
    const copyTodoList = [...todoList];
    copyTodoList.push(todo);
    this.todoList = copyTodoList
  };

  }
  ```

- 사실 계속 이렇게 값을 복사해서 써도 괜찮은가?
  - '이렇게 많이 복사해도 괜찮은가?'에 대해 물었을 때, 제이슨으로 부터 들은 말이 인상적이라서 덧붙인다.
  - `그런 관점에서 성능을 걱정한다면 우리는 변수 이름도 짧게 지어야 한다.`
  - `그런 컴퓨터 성능을 아끼는 것보다 개발자인 우리의 몸값이 더 비싸다.`

## 🚀 더 생각해볼 점

- `불변성`이 왜 중요한가?
