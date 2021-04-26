---
title: "자바스크립트 toFixed() 0 없애기"
date: 2021-04-26
categories: javascript
tags: javascript 자바스크립트 toFixed
---

자바스크립트에서 숫자를 정확한 자릿수로 표현하기 위해 [`toFixed()`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed){:target="\_blank"} 를 사용한다.<br>
`toFixed()` 는 지정한 자릿수보다 짧은 수일 때 뒤를 0으로 채우는데, 경우에 따라서 0을 제거할 경우가 생긴다.

> 1 이나 1.1 을 `toFixed(2)`로 처리를 하면 `"1.00"` `"1.10"` 으로 지정한 자릿수까지 0이 붙게 된다.<br>

0을 없애는 두 가지 방법이 있다.

- `parseFloat()` 사용

  ```javascript
  parseFloat(number.toFixed(2));
  ```

- 정규식 사용

  ```javascript
  number.toFixed(2).replace(/(0+$)/, "");
  ```
