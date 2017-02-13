---
layout: post
title:  "vue.js EventBus를 활용한 컴포넌트간 데이터 공유"
date:   2017-02-13 22:17:00 +0900
categories: jekyll update
author: "developerjin"
excerpt: "EventBus를 활용하여 Vue 혹은 Component간의 정보를 공유해보는 예제를 살펴봅니다."
---

프론트엔드 프레임워크를 사용하다보면 왠지 모르게 빡빡하게 짜여져 제공되는 것들 때문에 데이터의 흐름이나 여러 핸들링을 처리하기에 어려움을 겪기도 합니다.
제가 현재 작성하고 있는 `Vue.js`의 경우도 이러한 부분 때문에 클래식(?)스타일로 웹개발을 하시는 분들은 부담스러운 부분들도 있습니다.
데이터의 흐름을 쉽게 하기 위해 `vuex`라는 외부라이브러리가 있고, 메소드들을 서로 호출할 수 있도록 도와주는 `EventBus`라는 방법이 있습니다. 저는 이러한 것들 중에서 `EventBus`라는 것의 대해서 살펴보고 예제를 작성해보려 합니다.

## 이벤트를 사용하는 이유
일반적으로 메소드, 변수를 정의할 때 한 오브젝트나 컴포넌트 단위로 묶어서 사용되기 때문에 이벤트를 사용할 필요가 없이 현재위치에 포함된 메소드/변수를 호출하여 사용할 수 있습니다. 하지만 각각 분리되어 있는 개체에 전송하거나 알려줘야한다면 어떻게 해야할까요? 이럴때 공통으로 데이터들을 주고 받을 수 있는 공간을 만들고, 이를 통해서 서로 규격에 맞춰 데이터들을 주고 받으면 될 것 입니다. 과거 스마트폰이라는 개념이 나오기 전까지는 `푸시`라는 것이 생소하고 잘 사용하지도 않았으며, 이보다는 정기적으로 데이터를 긁어오는 `폴링`이라는 것이 더 사용했던 것 같습니다. 하지만 최근에는 `푸시`라는 용어와 함께 토픽, 구독, 발행등의 단어들을 사용하고 있는데 제 개인적으로는 이러한 것들이 이벤트와 비슷한 개념으로 받아들여도 되지 않을까라고 조심스럽게 의견을 내봅니다.
이벤트를 등록하고 받을 준비가 끝났다면 언제 어디서든지 데이터들을 주고 받고, 각 이벤트요청 상황에 따라 원하는 메소드들을 수행할 수 있을 것입니다.

## vue.js의 EventBus

vue.js에서 이벤트를 쉽게 다루기 위해 EventBus라는 개념을 이용할 수 있으며, 누구나 쉽게 사용할 수 있습니다.
```javascript
// 이벤트버스 생성
var EventBus = new Vue()
```

```javascript
// 이벤트 발행
EventBus.$emit('message', 'hello world');
```

```javascript
// 이벤트 구독
EventBus.$on('message', function(text) {
	console.log(text);
});
```

자, 이렇게 하여 vue.js에서 이벤트를 쉽게 활용할 수 있습니다. 위와 같이 구현해놓는다면 컴포넌트가 전혀 다르더라도 서로 쉽게 호출할 수 있습니다.

## 컴포넌트간 이벤트 주고받기

컴포넌트보다 한단계 위의 단계인 `VueApp`간의 이벤트 공유로 예제를 드리도록 하겠습니다. (제가 EventBus를 보면서 컴포넌트끼리 데이터주고 받는 것보다 극단적인 상황인 App단계에서의 이벤트공유가 가능여부가 더 궁금했었고, 이 글을 읽고 계신분들도 이 예제가 더 도움이 많이 될 것 같아 이렇게 진행했습니다.)

```xml
<div id="sender-app">
	<input v-model="text">
	<button @click="sender">sender</button>
    <div v-if="receiveText">#sender-app: I sent a message a {{ receiveText }}</div>
</div>
<div id="receiver-app">
	<div v-if="text">#receiver-app: {{ text }}</div>
</div>
```

```javascript
var EventBus = new Vue();

var SenderApp = new Vue({
	el: '#sender-app',
    data() {
    	return {
        	text: '',
            receiveText: ''
        }
    },
    created() {
    	EventBus.$on('message', this.onReceive);
    },
    methods: {
    	sender() {
        	EventBus.$emit('message', this.text);
            this.text = '';
        },
        onReceive(text) {
        	this.receiveText = text;
        }
    }
});

var ReceiverApp = new Vue({
	el: '#receiver-app',
    data() {
    	return {
        	text: ''
        }
    },
	created() {
    	EventBus.$on('message', this.onReceive);
    },
    methods: {
    	onReceive(text) {
        	this.text = text;
        }
    }
});
```

위 예제에서 확인했듯이 `SenderApp`과 `ReceiverApp`이 서로 다른 `VueApp`임에도 불구하고 `EventBus`를 통해 이벤트를 공유할 수 있습니다.

만약에 EventBus를 공용으로 사용하는 것이 아닌 Vue내에서만 사용하고 싶으시다면 아래와 같이 Vue내장함수로 사용할 수 있습니다.

```javascript
(function() {

	var EventBus = Vue();

    Object.defineProperties(Vue.prototype, {
        $eventBus: {
            get: function () {
                return EventBus;
            }
        }
    }
})();
```

관련 예제는 [Github](https://github.com/devjin0617/vuejs-eventbus-example)을 통해 공유되어 있으며, 궁금한 내용있으시면 [Github Issue](https://github.com/devjin0617/vuejs-eventbus-example/issues)를 통해 언제든지 알려주시면 감사하겠습니다.

## 참고자료

- [vue.js 공식홈페이지 - 비-부모-자식간-통신](https://kr.vuejs.org/v2/guide/components.html#비-부모-자식간-통신)
- [Building a Simple Event Bus in Vue.js - Arvid Kahl](https://devblog.digimondo.io/building-a-simple-eventbus-in-vue-js-64b70fb90834#.ksf302nhz)
- [vue.js 한국어 사용자 모임](https://vuejs-kr.github.io/)
- [진블로그 - Vuejs EventBus](http://blog.puding.kr/179)