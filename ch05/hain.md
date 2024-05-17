# 05. 더 좋은 액션 만들기

## 📝 핵심 내용

- 암묵적 입출력을 명시적으로 변경한다.
- 좋은 설계를 위해 구조를 잘 설계해야 한다. 구조를 명확하게 하기 위해서는 각 함수가 한 가지 일만 하도록 한다.

## 🤔 궁금한 점

- (89p.) 함수의 동작을 바꾸는 것은 리펙토링이 아니라고 한다. 그렇다면 만약 commit을 해야할 경우 fix 로 해야하는가?
  - (+) 단순히 함수를 분리하기만 하고, 분리한 함수의 이름을 정하는 건 리펙토링이 맞다고 생각하는데 다들 어떻게 생각하나요?

## 🚀 더 생각해볼 점

- 비즈니스 규칙과 관련된 것인지 아닌지 구분하는 이유
  - 👉비즈니스 규칙이 변경될 여지가 가장 많기 때문이 아닐까? 오늘 수업시간에 '도메인 객체'의 역할에 대해서 나왔는데, 비즈니스 규칙과 관련된 부분을 모으는 것이 도메인 객체와 비슷한 역할을 수행하는 것이라고 생각한다. 우리 서비스의 핵심 규칙이나 로직과 다른 것들을 분리하면, 변경사항이 생기더라도 도메인 객체 또는 비즈니스 규칙과 관련된 부분만 수정하면 금방 동작하게 할 수 있다는 장점이 있다.
- 아래 함수를 어떻게 분리해볼 수 있을까요?

```js
const RATING_STANDARD = 4;

function addWeeklyBestButton() {
  const movies = document.querySelectors(".movie-item");
  for (let i = 0; i < movies.length; i++) {
    const movie = movies[i];
    const movieInfo = getMovieInfo(movie);
    const movieRating = movieInfo.rating;
    if (movieRating > RATING_STANDARD) {
      movie.setWeeklyBest();
      const weeklyBestButton = document.createElement("button");
      weeklyBestButton.classList.add("weekly-best-button");
      movie.append(weeklyBestButton);
    }
  }
}
```
