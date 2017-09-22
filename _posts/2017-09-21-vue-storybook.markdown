---
layout: post
title: Storybook + Vue로 UI 컴포넌트 문서 만들기
date: 2017-09-21 12:00:00 +0900
categories: vue storybook
author: "ChangJoo Park"
excerpt: ""
---

이 문서는 UI 컴포넌트 개발을 위한 Storybook을 소개하고 Vue.js를 이용해 개발하는 방법을 다룹니다.
[공식 가이드](https://storybook.js.org/basics/guide-vue/)를 기반으로 설명합니다.

## Storybook 소개

![](https://storybook.js.org/basics/static/screenshot.png)

Storyboard는 상호작용이 가능한 UI 컴포넌트 뷰어를 만드는 도구입니다. 설치와 구성이 쉽고, 정적 웹사이트로 배포도 할 수 있습니다.

Storybook은 기존의 React만 사용할 수 있는 환경에서 최근에 Vue.js를 지원하기 시작했습니다. 특히 Vue.js 일본의 [Kazuya Kawakuchi](https://github.com/kazupon)의 노력이 있었습니다.

## Storybook for Vue

**순서**

- npm 프로젝트 생성
- @storybook/vue 설치
- vue 추가
- npm 스크립트 추가
- 설정 파일 추가
- 컴포넌트 추가
- 실행

Storybook + Vue는 `npm` 또는 `yarn`을 사용합니다. 여기서는 `npm` 을 사용하겠습니다.
아래로 나오는 `Storybook`은 `Storybook + Vue`를 말합니다.

### npm 프로젝트 생성

Storybook 프로젝트는 npm 을 사용합니다 아래 명령어로 프로젝트 디렉터리를 만들고, npm 프로젝트로 초기화합니다.

```
mkdir vue-storybook
cd vue-storybook
npm init -y
```


### @storybook/vue 설치

Storybook 저장소는 [이곳](https://github.com/storybooks/storybook) 입니다.
npm 명령어로 vue용 storybook 패키지를 설치합니다.

```
npm install --save-dev @storybook/vue
```

### vue 추가

@storybook/vue 패키지는 vue-cli를 기반으로 작동합니다. 하지만 Vue는 내장되어있지 않기 때문에 Vue 패키지를 설치합니다.

```
npm install --save vue
```

### 설정 파일 추가

Storybook을 실행하려면 설정파일이 필요합니다. Storybook의 설정파일은 `/.storybook/config.js` 경로에 있어야 합니다. 아래 명령어로 `config.js` 파일을 만듭니다.

```
mkdir .storybook
cd .storybook
touch config.js
```

config.js 파일의 내용은 아래와 같습니다.

```
// config.js
import { configure } from '@storybook/vue';

import Vue from 'vue';
import Vuex from 'vuex'; // Vue plugins

// Import your custom components.
import Mybutton from '../src/stories/Button.vue';

// Install Vue plugins.
Vue.use(Vuex);

// Register custom components.
Vue.component('my-button', Mybutton);

function loadStories() {
  // You can require as many stories as you need.
  require('../src/stories');
}

configure(loadStories, module);
```

### npm 스크립트 추가

storybook 프로젝트를 시작하려면 아래 명령어를 사용 해야 합니다.

```
start-storybook -p 9001 -c .storybook
```

npm 명령어로 간단히 실행하기 위해서 `package.json`에 storybook 스크립트를 추가합니다.

```
{
  "scripts": {
    "storybook": "start-storybook -p 9001 -c .storybook"
  }
}
```

이제 `npm run storybook`을 실행하면 `http://localhost:9001` 에서 Storybook 앱을 실행할 수 있지만 `Button.vue` 파일과 `loadStories`의 `/src/stories/index.js` 파일이 없어 오류가 발생합니다.

### 컴포넌트 추가

`config.js`에서 등록한 사용자 정의 컴포넌트를 선언하기 전에 `index.js`를 만듭니다.

```
// index.js
import Vue from 'vue';

import { storiesOf } from '@storybook/vue';

import MyButton from './Button.vue';

storiesOf('MyButton', module) // 상위 카테고리 지정
  .add('story as a component', () => ({ // 하위 내용 지정
    components: { MyButton },
    template: '<my-button :rounded="true">rounded</my-button>'
  }));
```

Storybook에서 사용할 첫번째 컴포넌트인 `Button.vue`를 만듭니다.

```
//Button,.vue
<template>
  <button class="{ rounded: rounded }"><slot></slot></button>
</template>

<script>
export default {
  props: {
    rounded: {
      type: Boolean,
      default: false
    }
  }
}
</script>

<style>
.rounded {
  border-radius: 5px;
}
</style>
```

### 실행

준비가 끝났으니 다시 `npm run storybook`을 실행합니다. webpack을 이용해 빌드가 끝나면 `http://localhost:9001` 로 접속하면 됩니다.

![Imgur](https://i.imgur.com/KHNx1fS.png)

MyButton 카테고리(Story)의 story as a component 를 보실 수 있습니다.

## 배포

Storybook 은 자체 빌드도구를 내장하고 있습니다. `package.json` 파일을 열어 `storybook` 스크립트 아래에 빌드용 스크립트를 추가합니다.

`"build": "build-storybook -c .storybook -o .out"`

터미널에서 `npm run build`를 입력하면 `.out` 디렉터리에 정적 페이지로 빌드됩니다. 빌드 된 결과를  [netlify](https://www.netlify.com/) 또는 Github pages로 배포하실 수 있습니다.

## 참고

- 공식 가이드 : https://storybook.js.org/basics/guide-vue/
- Story 상세 : https://storybook.js.org/basics/writing-stories/
- 예제 : https://storybook.js.org/examples/
