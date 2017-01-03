---
layout: post
title:  "jsFiddle에서 Vue.js 개발하기"
date:   2016-12-26 19:45:31 +0530
categories: update
author: "ChangJoo Park"
excerpt: "jsFiddle에서 Vue.js를 이용하여 Counter 앱을 만들어 봅니다"
---

# JSFiddle 환경에서 Vue 앱 시작하기

## JSFiddle

**JSFiddle** 은 간단한 웹 프로그래밍을 할 수 있는 브라우저 기반 에디터 입니다. 복잡한 앱을 만드는 경우 [vue-cli](https://github.com/vuejs/vue-cli)를 이용해서 작업을 하는 편이 훨씬 편리합니다. 하지만 간단한 기능 테스트에는 JSFiddle을 이용하는 것이 더 빠릅니다. 일단 설정해야 하는 양에서 있어 매우 큰 차이가 있습니다.

## Vue로 Counter 앱 만들기

### 1. 기본 앱 설정

이번에는 간단한 예제로 `counter`를 만들어 보겠습니다.

완성된 앱은 [여기](https://jsfiddle.net/changjoo_park/c05kmzzy/4/)에서 확인하실 수 있습니다.

첫번째로 Vue 애플리케이션 인스턴스를 만듭니다.


``` html
<div id="counter-app">
  {{count}}
</div>
```

``` js
const counterApp = new Vue({
  el: '#counter-app',
  data: function () {
    return {
      counter: 0
    }
  }
})
```

counterApp 은 Vue 인스턴스 입니다. `counter-app` ID를 가지는 div에 마운트됩니다. counterApp이 가지는 데이터 객체는 `counter` 입니다.
`counter` 객체는 `#counter-app` 안에서 Handlebars로 출력할 수 있습니다.


### 2. 증가/감소 메소드 추가

counterApp의 `count`를 증가/감소 시키는 메소드를 만들겠습니다.

``` html
<div id="counter-app">
  {{count}}
  <button v-on:click="increaseCount()">+</button>
  <button v-on:click="decreaseCount()">-</button>
</div>
```

``` js
const counterApp = new Vue({
  //...
  methods: {
    increaseCount: function () {
      this.count++
    },
    decreaseCount: function () {
      this.count--
    }
  }
})
```

html 에는 버튼을 두개 만들고 메소드를 연결했습니다. `increaseCount()`와 `decreaseCount()`는 Vue 인스턴스 메소드 입니다. `data`에서 추가한 `count`에 대한 증가/감소를 처리합니다.

### 3. count를 감시하는 computed

count가 5보다 이상 0보다 이하인 경우에 대한 처리는 어떻게 해야할까요? JavaScript에 익숙하면 methods의 각 메소드에 `if` 문을 이용하여 처리할 수도 있습니다. 하지만 이 조건이 앱의 여러 곳에서 필요한 경우 그때마다 `if`를 사용해야 합니다. vue.js는 이러한 코드 낭비를 줄이기 위해(그리고 성능에도 좋은) `computed`를 제공합니다.

``` html
<div id="counter-app">
  {{count}}
  <button v-on:click="increaseCount()" v-bind:disabled="isMoreThanEqualFive">+</button>
  <button v-on:click="decreaseCount()" v-bind:disabled="isLessThanEqualZero">-</button>
</div>
```

``` js
const counterApp = new Vue({
  //...
  computed: {
    isMoreThanEqualFive: function () {
      return this.count >= 5
    },
    isLessThanEqualZero: function () {
      return this.count <= 0
    }
  },
  methods: {
    increaseCount: function () {
     if (this.isMoreThanEqualFive) {
        return
      }
      this.count++
    },
    decreaseCount: function () {
     if (this.isLessThanEqualZero) {
       return
      }
      this.count--
    }
  }
})
```

이제 5이상이면 증가 버튼이 사용불가 상태가 되고, 0이하면 이하 버튼을 사용할 수 없게 됩니다. computed는 여러번 다시 사용해야하는 연산이 필요한 작업에 유용합니다.
`computed`, `watch`와 `methods`의  차이는 [vue.js 한글 문서 - 계산된 속성과 감시자](https://kr.vuejs.org/v2/guide/computed.html)를 읽어보세요.

## 마무리

이 예제는 설명을 위한 예제이기 때문에 약간은 과한 부분이 없지않아 있습니다. 하지만 이를 이해하면 기본적인 vue에 대해 약간은 알고 있다고 말할 수 있다고 생각합니다.
