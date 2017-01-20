---
layout: post
title:  "Vue.js 개발환경"
date:   2016-12-20 19:45:31 +0530
categories: update
author: "ChangJoo Park"
excerpt: "기본적인 개발환경 및 vue.js를 익히는 방법을 알아봅니다."
---

# vue.js 개발환경 이야기

이 페이지는 vue.js 개발환경에 대한 이야기를 합니다. 이 글을 읽는 모든 분들께 동일한 방법을 추천하는 것은 아닙니다. 이미 더 프론트엔드 지식이 많은 분들은 가볍게 훑어 보시기만 하면 됩니다. 프론트엔드 프레임워크 또는 라이브러리를 이용한 개발을 이제 막 시작하는 분께 설명하기 위해 **vue.js** 를 제외한 프레임워크/라이브러리에 대한 언급은 꼭 필요한 경우가 아니면 하지 않을 예정입니다.

자세한 사용법은 [vue.js 공식 가이드](http://vuejs.org/guide)를 보시거나 [vue.js 한국어 가이드](http://kr.vuejs.org/guide)를 보시면 됩니다.

## vue.js의 간단한 개발환경을 위한 JSFiddle

vue.js를 사용하기 위해 홈페이지에서 파일을 다운받고 에디터에서 설정하는 시간을 아끼기 위해서 [JSFiddle](https://jsfiddle.net/)을 이용합니다. 한 페이지에서 HTML, JS, CSS 를 모두 작성할 수 있고 결과도 바로 보여주기 때문에 가이드를 보고 따라하거나 간단한 양의 코드만 필요할 때 좋습니다.

vue.js은 버전업이 상당히 빠른 편입니다. 항상 최신 버전을 이용해서 테스트를 하고 싶은 경우 JSFiddle의 `External Resources` 메뉴에 `https://unpkg.com/vue/dist/vue.js` 이 주소를 추가하시면 됩니다. 아니면, `<script src="https://unpkg.com/vue/dist/vue.js"></script>`를 맨 위에 적어주셔도 상관없습니다.

추가하는 방법을 잘 모르시면 [여기](https://jsfiddle.net/9thLdb2v/)를 누르시면 vue.js 가 추가된 상태로 시작하실 수 있습니다.

vue.js 앱을 추가하는데 어려움이 있으시면 [여기서 시작](https://jsfiddle.net/9thLdb2v/2/) 하셔도 좋습니다.

jsfiddle에서 가이드만 따라가셔도 대부분의 기능들에 대해 이해하실 수 있습니다.

---

> 참고사항 : jsfiddle 이외에 브라우저에서 코드작성을 할 수 있는 서비스는 [codepen.io](http://codepen.io), [jsbin](http://jsbin.com/) 등이 있습니다. 각 서비스를 사용해 보신 후 마음에 드시는 곳에서 해도 무관합니다.

## vue.js와 vue-cli

vue.js를 사용하면서 가장 좋았던 것은 [vue-cli](https://github.com/vuejs/vue-cli)입니다. `vue-cli`를 사용해서 개발을 하면 vue.js의 컴포넌트 단위 개발에 대해 더욱 쉽게 다가가실 수 있습니다. `.vue` 파일은 template, js, css 파일을 모두 한 파일 내에서 관리할 수 있도록 유도합니다. 이 때문에 컴포넌트에 대한 대략적인 구상이 되면 빠르게 개발할 수 있습니다.

`vue-cli`는 [npm](https://www.npmjs.com/)을 이용해서 설치합니다. 자세한 설치 방법은 공식 [vue-cli 저장소](https://github.com/vuejs/vue-cli) 또는 [vue-cli 한국어 저장소](https://github.com/vuejs-kr/vue-cli)를 참고하세요.

### 템플릿 선택하기

크게 [Webpack](https://webpack.github.io/)을 이용하는 템플릿과 [Browserify](http://browserify.org/)를 이용하는 템플릿으로 나눌 수 있습니다. 아직 이 두개가 무엇인지 몰라도 괜찮습니다. 저는 Webpack을 추천합니다. [vuejs-templates 조직](https://github.com/vuejs-templates)에서 Webpack과 Browserify 저장소의 개발 현황만 봐도 Webpack 저장소가 훨씬 더 활발합니다. 저는 Webpack에 대한 지원이 잘 되고 있다고 판단되어 사용하고 있습니다. 아직 모를떄는 사람들이 많은 것을 사용해야 질문하기 편하다고 생각합니다.

JSFiddle을 사용하다 `vue-cli`를 사용하기 시작하면 많은 디렉터리와 파일에 어떤걸 먼저 보아야할지 막막해 질 수 있습니다. 포기하지 마시고 일단은 vue.js에 집중하기 위해 src 폴더만 바라보세요

### eslint에 대해

그리고 vue-cli에서 지원하는 강력한 기능인 `eslint`를 꼭 켜고 개발하시는 것을 추천합니다. eslint는 코드의 품질을 균일하게 만들어 줍니다. 처음에는 적응하기 힘들 수 있습니다. 규칙에 맞지 않는 코드를 작성하면 검증 단계에서 넘어가지 않아 추가한 코드가 적용 되었는지 확인할 수 조차 없습니다. 기본적으로 [Standard](https://github.com/feross/standard)와 [AirBNB](https://github.com/airbnb/javascript) 스타일을 제공합니다. 물론 스스로 설정할 수도 있습니다. 각 코드 스타일을 확인하고 취향에 맞는 것을 선택하세요.

### 테스팅에 대해

테스트는 vue.js를 처음 시작할 때 아직은 고려하지 않아도 되므로 선택하지 않아도 됩니다. vue.js는 아직 테스팅에 대한 자료가 많지 않습니다. [로드맵](https://github.com/vuejs/vue/projects/3)을 보면 향후 버전에서 컴포넌트 테스팅에 대한 유틸리티를 추가를 고려하고 있는 것을 알 수 있습니다.


### vue 컴포넌트와 텍스트 에디터

앞으로는 익숙하지 않은 `vue` 확장자를 가지는 파일을 가지고 작업을 해야합니다. 어떤 에디터를 사용하든 vue 커뮤니티에서 플러그인을 제공하고 있습니다.

- [Sublime Text의 Vue Syntax Highlight](https://packagecontrol.io/packages/Vue%20Syntax%20Highlight)
- [Atom의 language-vue](https://atom.io/packages/language-vue)
- [VS Code의 Vue Components](https://marketplace.visualstudio.com/items?itemName=seanwash.vue)

위 플러그인을 이용해 `vue`파일 작업을 하세요

## 확인해볼만한 링크 목록

주로 자주 들어가는 링크입니다.

- [vue.js 공식 홈페이지](https://vuejs.org/)
- [vue.js 한국어 홈페이지](https://kr.vuejs.org/)
- [vue.js 공식 채팅](https://gitter.im/vuejs/vue?utm_source=share-link&utm_medium=link&utm_campaign=share-link)
- [vue.js 한국어 사용자 채팅](https://vuejs-korea.signup.team/)
- [vue.js 공식 조직](https://github.com/vuejs)
- [vue.js 한국어 조직](https://github.com/vuejs-kr)
- [awesome vue](https://github.com/vuejs/awesome-vue)
- [vue.js 포럼](http://forum.vuejs.org/)

공식 채팅방은 매우 활발한 편이고 코드만 잘 전달해주면 대부분 친절하게 대답해줍니다. 막히는 부분이 생겼을 때 가장 도움이 많이 되는 곳 입니다. 한국어 사용자 채팅방도 활성화되어 잘하는 분들께서 많이 오셔서 도와주시기를 기대하고 있습니다.


