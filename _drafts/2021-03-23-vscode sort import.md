---
title: "vscode javascript sort import 자동정렬"
date: 2021-03-23
categories: javascript
tags: javascript vscode sort import
---

코드에 `import` 를 추가 할 때도, 어느 위치에 넣을지 은근히 신경쓰이기 때문에, 자동으로 정렬해주는 기능을 좋아한다. 여러 방법이 있겠지만, 플러그인을 사용하는 방법과 vscode 설정으로 처리하는 2가지 방법을 적어본다.

# sort-imports

[sort-imports](https://marketplace.visualstudio.com/items?itemName=amatiasq.sort-imports) 플러그인 설치 시 자동으로 import 정렬이 된다.

정렬 전
![정렬 전](/assets/img/sortimports_0.png)

정렬 후
![정렬 후](/assets/img/sortimports_1.png)

# vscode 설정

`settings.json` 파일에 아래 내용을 추가 하면 파일 저장시에 자동 정렬이 된다.

```json
"editor.codeActionsOnSave": {
  "source.organizeImports": true,
}
```

설정 - 텍스트편집기 - Code Actions On Save - 'settings.json에서 편집' 버튼을 눌러서 파일로 들어 갈수도 있다.
![설정](/assets/img/sortimports_2.png)
