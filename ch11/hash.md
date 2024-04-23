# 11장 일급함수 2

고차함수 만들기

적절한곳에서 고차함수 만드는 법!!!

10장과 내용이 동일합니다~~~☺️

```js
//고차함수 사용 예시1

async function executeRetry(asyncFunc) {
  try {
    return await asyncFunc();
  } catch (error) {
    return alert(error.message);
  }
}

export default executeRetry;

-------------------------------
//활용예시
  #lottoPurchaseHandler(e) {
    executeRetry(async () => {
      e.preventDefault();
      await this.#lottoPurchase();

      outputView.showAfterBuyLottos(this.#lottoCount, this.#generatedLottos);
    });
  }

```
