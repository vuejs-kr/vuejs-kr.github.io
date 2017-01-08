---
layout: post
title:  "vue와 vue-router, vuex 함께 사용하기"
date:   2017-01-08 08:45:31 +0530
categories: update
author: "ChangJoo Park"
excerpt: "vue의 컴패니언 라이브러리를 사용하는 방법을 빠르게 살펴봅니다."
---

# Vue의 컴패니언 라이브러리 (vue-router, vuex)

이 글은 vuex 와 vue-router를 **적용**하는 방법에 대해서 알아봅니다. 매우 간략한 내용만 다루고 있으므로 더 자세한 사용 방법에 대한 내용은 [vuex 한국어 문서](https://vuex.vuejs.org/kr/)와 [vue-router 한국어 문서](https://router.vuejs.org/kr/)를 읽어보세요.

## vue-cli 

vue-cli는 기본적으로 vue만 포함하고 있습니다. [vuejs 템플릿 github 조직](https://github.com/vuejs-templates)은 vue-cli의 템플릿을 간결하게 유지하고 싶기 때문에 vuex 또는 vue-router를 포함하려 하지 않고 있습니다. 이와 관련한 논의는 [이 이슈](https://github.com/vuejs-templates/webpack/pull/347)와 [이 이슈](https://github.com/vuejs-templates/webpack/pull/296#discussion_r82073211)를 읽어보세요. 

하지만 매번 vuex와 vue-router를 추가하는 것은 매우 까다로운 일이기 때문에 템플릿을 만들었습니다. [이 저장소](https://github.com/ChangJoo-Park/webpack)를 살펴보세요. 간단한 사용법은 아래에 있습니다. 수정하려면 fork 하여 사용하시면 됩니다. 현재 standard와 airbnb 스타일을 모두 지원하고 있습니다.

```terminal
$ npm install -g vue-cli
$ vue init changjoo-park/webpack my-project
$ cd my-project
$ npm install
$ npm run dev
```

자신만의 템플릿을 만드는 것은 [사용자 정의 템플릿 작성해보기](https://github.com/vuejs-kr/vue-cli#사용자-정의-템플릿-작성해보기)를 읽어보세요

## vue-router 와 vuex

### 프로젝트 구조

위 템플릿으로 만든 `my-project`를 살펴보겠습니다. 

![폴더 구조](http://i.imgur.com/3szmkHP.png)

vue-cli로 만든 `my-project` 앱의 `src` 디렉터리의 구조입니다.  추가된 것은 `routes.js`와 `vuex` 디렉터리입니다.

### vue-router

`main.js` 를 살펴보겠습니다.

```javascript
// The Vue build version to load with the `import` command
// (runtime-only or standalone) has been set in webpack.base.conf with an alias.
import Vue from 'vue'
import Router from 'vue-router'
import App from './App'
import routes from './routes'
// Styles
import './assets/styles/styles.scss'

Vue.use(Router)
const router = new Router({
  mode: 'history',
  scrollBehavior: () => ({ y: 0 }),
  routes
})

/* eslint-disable no-new */
new Vue({
  router,
  ...App
}).$mount('#app')
```

중요한 부분은 **4번**, **6번**, **10번 ~ 15번**, **19번** 코드 입니다.

vue-router와 라우트에 대한 내용(routes.js)을 가져오고(**4번**, **6번**), Vue에 vue-router를 사용할 것을 알려줍니다.(**10번**) 그리고 router 인스턴스를 만든 후(**11번** ~ **15번**) 이 것을 실제 사용할 Vue에 넣어 인스턴스를 만듭니다.(**19번**)

**11번** ~ **15번** 코드의 `mode`나 `scrollBehavior`는 공식 문서의 [HTML5 히스토리 모드](https://router.vuejs.org/kr/essentials/history-mode.html)와 [스크롤 동작](https://router.vuejs.org/kr/advanced/scroll-behavior.html)을 읽어보세요.

`main.js` 파일에서 살펴보지 않은 `routes.js` 파일을 살펴보겠습니다.

```javascript
import HomePage from 'components/HomePage'
import CounterPage from 'components/CounterPage'

export default [
  {
    path: '/',
    name: 'home-page',
    component: HomePage
  },
  {
    path: '/counter',
    name: 'counter-page',
    component: CounterPage
  },
  {
    path: '*',
    redirect: '/'
  }
]
```

앞에서 설명하지 않았지만 `changjoo-park/webpack` 템플릿에는 **HomePage** 와 **CounterPage** 컴포넌트가 포함되어 있습니다.

`path`와 `name` 그리고 `component`를 지정해주면 해당 `path`에 맞는 컴포넌트를 화면에 그려줍니다. 지정하지 않은 `path`를 걸러내기 위해 맨 아래 라우트를 추가하였습니다. 자세한 내용은 공식 vue-router 가이드의 [시작하기](https://router.vuejs.org/kr/essentials/getting-started.html)를 보시면 됩니다.

`App.vue` 단일 파일 컴포넌트의 템플릿을 보겠습니다.

```html
<template>
  <div id="app">
    <ul class="navigation">
      <li><router-link v-bind:to="{ name: 'home-page' }">Home</router-link></li>
      <li><router-link v-bind:to="{ name: 'counter-page' }">Counter</router-link></li>
    </ul>
    <router-view></router-view>
  </div>
</template>
```

`App.vue` 템플릿에서 보셔야할 부분은 `router-link`와 `router-view` 입니다. `router-link`는 라우트를 지정해주고, `router-view`는 지정된 라우트에 맞는 컴포넌트를 화면에 그려줍니다.

#### 새 페이지를 추가하려면

새 페이지를 추가하는 방법은 다음 순서로 하시면 됩니다.

- 새 페이지를 위한 컴포넌트를 만듭니다.
- `routes.js`에 라우트를 정의해 줍니다.
- 새로 만든 페이지로 이동할 수  있도록 `router-link`를 만듭니다.



### vuex

vuex 는 앱 전체에서 공유할 수 있는 데이터센터 입니다. 부모 컴포넌트와 자식 컴포넌트 사이에 데이터를 주고 받을 때 불편함을 해소하는데 큰 역할을 합니다.

vuex 디렉터리 구조 입니다.

![](http://i.imgur.com/bpfZUTz.png)

vuex의 자세한 내용은 [vuex 시작하기](https://vuex.vuejs.org/kr/intro.html)를 읽고 보시면 더 이해가 쉽습니다. vuex의 `main.js`파일에 해당하는 `store.js` 부터 살펴보겠습니다.

```javascript
import Vue from 'vue'
import Vuex from 'vuex'
import * as actions from './actions'
import * as getters from './getters'
import modules from './modules'

Vue.use(Vuex)

export default new Vuex.Store({
  actions,
  getters,
  modules,
  strict: true
})
```

vue-router를 추가하는 것과 동일한 흐름으로 이해하시면 됩니다. vuex 구조에서 `actions`, `getters`(state), `modules`(mutations)를 Vuex의 `store`에 추가하여 vuex인스턴스를 만드는 코드입니다.

![vuex 구조](https://vuex.vuejs.org/kr/images/vuex.png)



우선 `mutation-types.js`를 보겠습니다. vuex가 어떤 종류의 `mutation`을 정의한 파일입니다.

```javascript
export const DECREMENT_MAIN_COUNTER = 'DECREMENT_MAIN_COUNTER'
export const INCREMENT_MAIN_COUNTER = 'INCREMENT_MAIN_COUNTER'
```

카운터를 증가시키고 감소시키는 조작만 있으면 됩니다.

이제 실제 조작에 대한 구현을 보겠습니다. `counters.js` 파일입니다. modules 디렉터리의 `index.js`는 modules에 있는 module을 export 해주는 유틸 스크립트입니다. 한번 살펴보세요.

```javascript
import * as types from '../mutation-types'

const state = {
  main: 0
}

const mutations = {
  [types.DECREMENT_MAIN_COUNTER] (state) {
    state.main--
  },
  [types.INCREMENT_MAIN_COUNTER] (state) {
    state.main++
  }
}

export default {
  state,
  mutations
}
```

`state`가 있고, `mutations`가 있습니다. state는 실제 counter의 상태를 보관합니다. 그리고 mutations는 위에서 정의한 조작 요청이 오면 카운터를 증가 / 감소 시킵니다.

mutation을 실제로 발생시키려면 action이 필요합니다. action은 vue 컴포넌트에서 `dispatch`를 이용해 호출하는 메소드입니다.

```javascript
import * as types from './mutation-types'

export const decrementMain = ({ commit }) => {
  commit(types.DECREMENT_MAIN_COUNTER)
}

export const incrementMain = ({ commit }) => {
  commit(types.INCREMENT_MAIN_COUNTER)
}
```

`actions`는 백엔드 API와 통신하는 역할을 가지지만 여기서는 백엔드가 없는 앱이므로 바로 mutation을 `commit`을 이용해 요청합니다.

이제 컴포넌트에서 사용할 상태를 호출하기 위한 `getters`(state) 를 살펴보겠습니다. 파일은 `getters.js` 입니다.

```javascript
export const mainCounter = state => state.counters.main
```

템플릿에 포함된 애플리케이션은 Counter 앱에 대한 상태만 보관하므로 **mainCounter**라는 **getter**만 있습니다 .state의 counters의 main을 반환하는 역할을 합니다. 이 곳에 `filter`나 `map`등을 이용해 원하는 내용만 사용할 수 있습니다.

vuex가 준비되었으므로 counter 앱을 만들어보겠습니다. 가장 먼저, App 컴포넌트에 vuex를 사용한다고 알려줘야 합니다.

`App.vue`의 `script` 블럭입니다.

````javascript
<script>
import store from 'src/vuex/store'

export default {
  store,
  name: 'app'
}
</script>
````

vuex의 store를 가져와 App 컴포넌트에 추가했습니다. 이제 앱의 어디든 `this.$store`를 이용해 vuex에 접근할 수 있습니다.

`CounterPage.vue` 파일에 카운터 앱이 있습니다. 따로 컴포넌트로 분리해 보세요. 템플릿을 만들기 쉽게하기 위해 `CounterPage.vue`에 넣은 것 입니다. 코드를 살펴보겠습니다.

템플릿 코드 입니다.

```html
<div class="counter-number">
  {{counter}}
</div>
<button class="counter-button" v-on:click="incrementCounter">+</button>
<button class="counter-button" v-on:click="decrementCounter">-</button>
```

스크립트 코드입니다.

```javascript
computed: {
  counter () {
    return this.$store.getters.mainCounter
  }
},
methods: {
  incrementCounter () {
    this.$store.dispatch('incrementMain')
  },
  decrementCounter () {
    this.$store.dispatch('decrementMain')
  }
}
```

`computed`에 `getters`에  지정해 놓은 **mainCounter**를 가져왔습니다. 그리고 `mehotds`에는 `actions`에 추가한 액션들을 호출하는 코드가 있습니다. action을 실행하는 것은 `dispatch`입니다. 자세한 내용은 [Vuex가이드의 액션](https://vuex.vuejs.org/kr/actions.html)을 읽어보세요.

#### 새 모듈을 추가하려면

- mutation-types.js 에 어떤 변이가 필요한지 추가하세요.
- modules 디렉터리에 사용할 모듈 파일을 만들고, state와 mutations을 추가하세요.
- `actions.js`에 mutation을 호출할 메소드를 만드세요.
- `getter.js`에 컴포넌트에서 사용할 상태를 추가하세요.
- 컴포넌트에서 `computed`에 상태를, `methods`에 액션을 호출할 `dispatch`를 추가하세요.

[vuex의 가이드](https://vuex.vuejs.org/kr/intro.html)에 더 자세한 내용이 있습니다. 꼭 읽어보세요.