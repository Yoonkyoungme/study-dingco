## 3월 8일 스터디 정리

### 기본 용어

- 액션 : 실행 시점과 횟수에 의존. 부수효과가 있는 함수.
  ex) 이메일 보내기, 디비 읽기

- 계산 : 입력으로 출력 계산. 순수함수,
  ex) 최댓값 찾기, 이메일 주소 옳은지 확인하기

- 데이터 : 이벤트에 대한 사실. 일어난 일의 결과
  ex) 사용자가 입력한 이메일 주소..

### 데이터

함수형 프로그램에서 중요한 부분은! **"데이터를 언제나 쉽게 해석할 수 있도록 표현하는 것"**

#### 함수형 프로그래밍에서의 데이터

- 불변성 : 카피 온 라이트(변경할 때 복사본 생성)
  방어적 복사(보관하려는 데이터의 복사본을 생성)
- 데이터의 장점 : 데이터 자체로 이해 가능.
  - 직렬화 :
  - 동일성 비교 : 비교가 쉬움.
  - 자유로운 해석 :
- 데이터의 단점 : 유연하게 해석이 가능하나 반드시 해석이 필요함.

함수형 프로그래머는 일반적으로 가능하면 액션을 쓰지 않으려고 함. 가능한 계산을 사용하려고 함. => 테스트하기 쉽기 때문,, 계산을 나누면 구현하기 쉬움.

데이터 => 계산 => 액션 순으로 구현하는게 일반적이다!