---
layout: post
title:  "Vue.js 플러그인 제작 가이드"
date:   2017-01-13 15:53:31 +0900
categories: jekyll update
author: "Lee Sun-Hyoup"
excerpt: "Vue.js에서 플러그인 제작을 예제를 통해 해봅시다."
---

Vue.js는 외부 라이브러리를 쉽게 불러올 수 있도록 표준 플러그인 기능을 제공합니다. `vuex`, `vue-router` 같은 공식적으로 제공해주는 라이브러리도 이 플러그인 기능을 통해 제작되었습니다. 직접 만든 라이브러리를 배포할 수 있도록 플러그인 기능에 대해서 알아봅시다. :)

## 플러그인 사용법
먼저 플러그인은 **제공자**와 **사용자**로 나뉩니다. 이때 두 사이를 이어주는 것이 Vue의 `install` 메서드입니다. 아래는 각각 예제 코드입니다.

### 제공자 코드
```js
// my-plugin.js
MyPlugin.install = function (Vue, options) {
  /* ... */
}
```

### 사용자 코드
```js
import MyPlugin from 'my-plugin';
Vue.use(MyPlugin);
```

엄청 간단합니다! `install` 메서드에는 **제공자**가 제공하고자 하는 기능을 작성하면 됩니다. 공식 홈페이지의 예제를 보면,

```js
MyPlugin.install = function (Vue, options) {
  Vue.myGlobalMethod = function () {
    // 필요한 로직 ...
  }
  
  Vue.directive('my-directive', {
    bind (el, binding, vnode, oldVnode) {
      // 필요한 로직 ...
    }
    ...
  })
  
  Vue.mixin({
    created: function () {
      // 필요한 로직 ...
    }
    ...
  })
  
  Vue.prototype.$myMethod = function (options) {
    // 필요한 로직 ...
  }
}
```
전역 메소드 추가, 전역 디렉티브 추가, 믹스인, 혹은 인스턴스 메소드를 추가할 수 있습니다. Vue에 관련된 것만이 아니라 그 외의 코드도 작성이 가능합니다.

## 실사용 예제
저는 이미 실제 프로젝트에서 몇 가지 플러그인을 제작한적이 있습니다. 아래 예제는 제가 만들었던 플러그인 중 하나인 모달 플러그인입니다.

### 플러그인 vue 파일
```html
<!-- alert-modal-template.vue -->
<template>
    <transition name="modal">
        <div class="modal-mask" v-if="isShowModal">
            <div class="modal-wrapper">
                <div class="modal-container">
                    <div class="modal-header">{{title}}</div>
                    <div class="modal-body">{{message}}</div>
                    <div class="modal-footer">
                        <button class="modal-default-button" @click="close">확인</button>
                    </div>
                </div>
            </div>
        </div>
    </transition>
</template>

<style scoped>
    .modal-mask {
        position: fixed;
        z-index: 9998;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: rgba(0, 0, 0, .5);
        display: table;
        transition: opacity .3s ease;
    }

    .modal-wrapper {
        display: table-cell;
        vertical-align: middle;
        text-align: center;
    }

    .modal-container {
        width: 300px;
        margin: 0px auto;
        padding-top: 50px;
        background-color: #fff;
        border-radius: 2px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, .33);
        transition: all .3s ease;
        font-family: Helvetica, Arial, sans-serif;
    }

    .modal-header {
        margin-top: 0;
        font-weight: bold;
        font-size:0.95em;
    }

    .modal-body {
        margin: 16px 0 50px 0;
        font-size:0.95em;
    }

    .modal-default-button {
        width: 100%;
        height: 50px;
        text-align: center;
        background-color: #333333;
        border: none;
        color: white;
        font-size:0.95em;
    }

    .modal-enter {
        opacity: 0;
    }

    .modal-leave-active {
        opacity: 0;
    }

    .modal-enter .modal-container,
    .modal-leave-active .modal-container {
        -webkit-transform: scale(1.1);
        transform: scale(1.1);
    }
</style>


<script>
    export default {
        methods: {
            show: function(title, message) {
                this.isShowModal = true;
                this.title = title;
                this.message = message;
            },
            close: function() {
                this.isShowModal = false;
            }
        },
        data() {
            return{
                isShowModal: false,
                title: '알림',
                message: ''
            }
        }
    }
</script>
```

### 플러그인 js 파일
```js
// alert-modal.js
import AlertModalTemplate from './alert-modal-template.vue';

module.exports = {
    install(Vue) {
        var ModalConstructor = Vue.extend(AlertModalTemplate);
        var modalInstance = null;

        window.AlertModal = function () {
        };

        window.AlertModal.show = function (title, message) {
            if (modalInstance) {
                modalInstance.isShowModal = true;
                modalInstance.title = title;
                modalInstance.message = message;
                return;
            }

            modalInstance = new ModalConstructor({
                el: document.createElement('div'),
                data() {
                    return {
                        title: title,
                        message: message
                    };
                }
            });
            modalInstance.isShowModal = true;
            document.body.appendChild(modalInstance.$el);
        };

        window.AlertModal.close = function () {
            if (modalInstance) {
                modalInstance.isShowModal = false;
                return;
            }
        }
    }
};
```

### 사용자 코드
```js
import AlertModal from 'alert-modal'

Vue.use(AlertModal)
AlertModal.show('알림', 'Hello, Vue.js!')

```

## 더 나은 시작을 위한 도구
저는 간단한 플러그인을 제작했고 사내에서만 사용하기 때문에 플러그인 제작을 간단하게 했지만 만약 테스트, Lint와 같은 도구를 추가한다면 플러그인 작성이 힘들어질 수 있습니다. 그래서 이미 플러그인 제작을 위한 [vue-plugin-boilerplate](https://github.com/kazupon/vue-plugin-boilerplate)라는 `vue-cli` 템플릿이 등장했습니다. `vue-plugin-boilerplate`는 플러그인 제작을 위한 README와 같은 문서 작성, e2e 테스트, 유닛 테스트, Lint 등 다양한 작업을 미리 셋팅해주는 오픈소스입니다. 다음은 `vue-plugin-boilerplate`가 사용하는 툴입니다.

* Transpiler / Compiler
  * babel (for development)
  * buble (for distribution)
* Type Checker
  * flowtype
* Linter
  * eslint
* Bundler
  * webpack (for development)
  * rollup (for distribution)
* Test Assertion
  * power-assert
* Test Framework
  * jasmine
* Test Runner
  * karma
* Test Coverage
  * istanbul
* Headless Browser
  * phantomjs
* End-to-End Test Fremework
  * nightwatch.js

사용자는 여기서 사용할 것과 사용하지 않을 것을 init할때 설정할 수 있습니다.

## 마치며..
Vue.js는 아직 성장중은 오픈소스입니다. 만약 플러그인을 제작했다면 Vue.js 커뮤니티의 발전을 위해서 공개해보는건 어떨까요? :)