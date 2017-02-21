---
layout: post
title:  "VSCODE Sass, Vue, Vue Snippet 세팅하기"
date:   2017-02-21 22:17:00 +0900
categories: jekyll update
author: "jinhan"
excerpt: "VSCODE에서 Vue개발환경 세팅을 해봅시다."
---

# VSCode Sass, Vue 세팅하기

항상 `Vue.js korea`에서 많은분들의 도움을 받고있습니다. 감사합니다~

어제 `vscode`를 처음 써보면서 세팅하면서 겪었던 시행착오들을 한번 정리해봤습니다.

`vscode`를 처음 쓰시고자 하는분들한테 도움이 되길 바랍니다.

## Sass
vscode를 맨처음 설치하고 단일 파일 `*.vue`로 된파일을 열었을때 `scss`가 있으면 아래와 같이 에러가 발생합니다.

![](https://s3.ap-northeast-2.amazonaws.com/leoheo-resource/Pasted+image+at+2017_02_17+03_54+PM.png)

이 에러가 발생하는 이유는 기본적으로 vscode가 `sass`를 지원하지 않기 때문입니다.

그래서 `sass를 사용하기 위해서는 extension설치와 설정을 해주어야 합니다.`

`설치 후 설정 중요!!`

먼저 vscode에서 `vue`파일을 사용하기 위해서 제가 사용한 extension은 [Vue Components](https://marketplace.visualstudio.com/items?itemName=seanwash.vue) 입니다.

vscode에서 extension를 쉽게 설치 하기 위해서 제공되는 편의기능중에 `F1`를 누르고 나오는 입력창에서 아래와 같이 입력하면 됩니다.

![](https://s3.ap-northeast-2.amazonaws.com/leoheo-resource/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA+2017-02-18+%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE+1.51.51.png)

그러고 나서 `Vue Components`를 설치해줍니다.

![](https://s3.ap-northeast-2.amazonaws.com/leoheo-resource/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA+2017-02-18+%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE+1.54.29.png)

여기까지 설치가 다 되어서 다시 파일을 가보면 vscode는 여전히 에러를 뱉어내고 있습니다.

여기서 아까 말한 `설정`이 필요합니다.

`README`를 보면 아래와 같이 설정하라고 나옵니다.

![](https://s3.ap-northeast-2.amazonaws.com/leoheo-resource/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA+2017-02-18+%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE+1.58.36.png)

> 그러면 이 설정을 어떻게 하느냐?!에서 많은 분들이 궁금증을 가지실수 있습니다.

그래서 vscode는 이러한 세팅을 `settings.json`라는 파일로 세팅을 관리합니다.

프로젝트의 root에 `.vscode`라는 디렉토리를 찾으실수 있을겁니다.

![](https://s3.ap-northeast-2.amazonaws.com/leoheo-resource/Pasted+image+at+2017_02_17+03_58+PM.png)

그래서 `settings.json`파일을 열고 아까 본 설정을 적어줍니다.

```json
"files.associations": {
  "*.vue": "vue"
}
```

이렇게 적용을 하면 인제부터 `.vue`파일에서 `scss`를 사용할 수 있습니다.

![](https://s3.ap-northeast-2.amazonaws.com/leoheo-resource/Pasted+image+at+2017_02_17+03_55+PM.png)

## Vue
vscode에서 `eslint`를 적용하기 위해서 저는 `vscode-eslint`를 설치하였습니다.

이 가이드는 javasript lint중에 `standard`를 기준으로 설명하겠습니다.

[vscode-eslint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)를 설치 하고 아무런 세팅을 안해주면

`js에서는 잘 되는데 vue파일에서는 linting이 작동하지 않았습니다.`

그래서 `vscode-eslint` 이슈를 살펴보던 중에 해결방법을 찾았습니다.

`vscode-eslint`를 설치하고 나면 기본적인 세팅은 2가지만 되어있습니다.

```json
// An array of language ids which should be validated by ESLint
"eslint.validate": [
  "javascript",
  "javascriptreact"
],
```

그래서 작업하시는 프로젝트의 `settings.json`에 아래와 같이 적용해줍니다.

```json
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "html",
    "vue"     
  ]
```

이 방법은 [vscode-eslint의 ISSUE #42](https://github.com/Microsoft/vscode-eslint/issues/42#issuecomment-264836417)를 참고하였습니다.

이렇게 세팅을 하고 나면 인제 `.vue`파일에서도 linting이 되는걸 확인하실수 있습니다.

![](https://s3.ap-northeast-2.amazonaws.com/leoheo-resource/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA+2017-02-18+%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE+2.30.46.png)

지금까지 vscode에서 `sass, vue세팅하기`에 대해서 알아봤습니다.

## 부록
저처럼 `귀차니즘이 심한 개발자`라서 어떻게 하면 좀 더 자동화를 해볼까? 라고 생각하시는 분이 있다면 `vue snippet`를 등록해보세요.

![](https://s3.ap-northeast-2.amazonaws.com/leoheo-resource/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA+2017-02-22+%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB+12.23.09.png)

![](https://s3.ap-northeast-2.amazonaws.com/leoheo-resource/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA+2017-02-22+%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB+12.23.19.png)

위와같은 절차로 들어가게 되면 `vue.json`파일이 나옵니다. 거기에 아래 `snippet`를 추가해주세요.

```json
"init template": {
	"prefix": "tpl",
	"body": [
		"<template lang='$1'>",
		"",
		"</template>",
		"<script>",
		"export default {",
		"",
		"}",
		"</script>",
		"<style lang='$2'>",
		"",
		"</style>",
		""
	],
	"description": "single vue file initial template"
}
```

그러면 아래와 같이 사용할수 있습니다.

![](https://s3.ap-northeast-2.amazonaws.com/leoheo-resource/ezgif.com-video-to-gif+(3).gif)
 
마지막으로 틀린점이나 조금 더 나은방법이 있으신분은 언제나 지적해주시면 감사하겠습니다 ^^
