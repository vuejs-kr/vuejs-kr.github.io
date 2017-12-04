---
layout: post
title: "Vue.js 공식 ESLint 플러그인 적용하기"
date: 2017-12-03 12:00:00 +0900
categories: vue eslint
author: "ChangJoo Park"
excerpt: "webpack 템플릿에 eslint-plugin-vue를 적용해봅니다."
---

Vue.js는 사용하는 사람에 따라 코딩 스타일이 매우 다릅니다. 공식문서는 잘 되어있지만 실제 작업하다보면 규칙을 잘 지키기 힘듭니다.

가장 많이 사용하는 Vue.js 템플릿 중 [webpack
템플릿](https://github.com/vuejs-templates/webpack)은 두가지 코딩 규칙(미적용 제외)을 지원합니다. 저의
경우 [Standard](https://standardjs.com/) 규칙을 사용하고 있고 여러 프로젝트에서 사용하면서 자연스럽게
적응되었습니다. [Vue.js 공식 스타일 가이드](https://kr.vuejs.org/v2/style-guide/)가 나왔지만 하나하나
문서를 보면서 외우기는 너무 어려운 일이라고 생각합니다. Standard 처럼 사용하면서 적응하려면 어떻게 해야할까요?

얼마전 Vue팀은 [eslint-plugin-vue](https://github.com/vuejs/eslint-plugin-vue)를
공개하였습니다. 아직 베타지만 스타일가이드를 따르면서 Vue.js 프로젝트를 할 수 있는 정도로 개발된 상태입니다. 로컬 개발환경에서 사용하기
전에 [웹](https://mysticatea.github.io/vue-eslint-demo/)에서 미리 체험할 수 있습니다.

이 글은 webpack 템플릿에서 eslint-plugin-vue를 적용하는 방법을 알아봅니다.

—

`vue init webpack my-project`

터미널에서 위 명령어로 새 프로젝트를 만듭니다.

eslint 설정을 담고 있는 파일은 **.eslintrc.js **입니다. 기본값은 아래와 같습니다.

<script src="https://gist.github.com/ChangJoo-Park/0d19afe8bcaeee3ed2aeb48ad81ab1ac.js"></script>

최종 설정입니다.

<script src="https://gist.github.com/ChangJoo-Park/b31b1bdc60ea73147a73a2cb2d006b1a.js"></script>


터미널 작업 내용을 보려면 아래의 링크를 이용하세요


<script type="text/javascript" src="https://asciinema.org/a/150810.js" id="asciicast-150810" async></script>


위의 설정을 .eslintrc.js 에 넣으면 스타일 가이드의 추천 설정으로 코드를 검증할 수 있습니다. webpack 템플릿의
보일러플레이트에서 스타일 가이드에 어긋난 부분을 찾아보세요.

—

**주의사항 : ** parser 속성이 parserOptions안에 있어야 합니다. 그렇지 않으면 eslint가 정상적으로 작동하지 않습니다.

Vue 스타일 가이드는 4단계의 규칙 범위를 나누었습니다. eslint-plugin-vue 기준을 설명합니다.

* `plugin:vue/base` - 기본적인 ESLint의 파서를 실행합니다.
* `plugin:vue/essential` - base 규칙과 함께 에러를 내는 코드와, 예상치 못한 결과를 낳는 코드를 예측합니다.
* `plugin:vue/strongly-recommended` - 위의 내용과 함께 코드 가독성 및 개발 경험을 높일 수 있는 규칙을 적용합니다.
* `plugin:vue/recommended` - 위 내용과 함께, 일관성을 위한 커뮤니티에서 추천하는 규칙을 적용합니다.

.eslintrc.js 에서 `plugin:vue/recommended` 를 `plugin:vue/base` 등으로 변경하면 다른 규칙을
적용하여 프로젝트를 진행하실 수 있습니다.

—

[스타일 가이드의 한국어 번역](https://kr.vuejs.org/v2/style-guide/)은 현재 베타버전이므로 제목 정도만 번역해둔
상태입니다. 한글화에 참여하려면 [이곳](https://github.com/vuejs-kr/kr.vuejs.org)을 참고하세요. Pull
Request는 항상 환영합니다.
