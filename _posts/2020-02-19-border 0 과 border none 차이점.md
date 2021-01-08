---
title: "[css] border: 0; 과 border: none; 차이점"
date: 2020-02-19
categories: css
tags: css
---

[Airbnb CSS / Sass 스타일 가이드](https://github.com/CodeMakeBros/css-style-guide#user-content-border) 에서 Border 부분을 보다가 궁금한 부분이 생겼다.

> ## Border  
> border가 없을 경우에는 `none` 대신 `0`을 사용하세요  
> <br>
> **잘못된 예시**
> ```
> .foo {
>   border: none;
> }
> ```
> <br>
> **올바른 예시**
> ```
> .foo {
>   border: 0;
> }
> ```

관련된 내용의 포스팅을 발견해서 남겨본다.  
[border:none ? border:0 !](https://trend21c.tistory.com/287)

간단히 얘기해서, `border:` 뒤에는 `border-width` 값이 와야되는데 `none` 이 올 경우, 특정 환경에서 동작을 안 할 수도 있다는 내용.


