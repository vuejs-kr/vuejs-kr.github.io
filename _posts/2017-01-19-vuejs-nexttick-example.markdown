---
layout: post
title:  "Vue.js에서 nextTick 사용하기"
date:   2017-01-19 21:36:00 +0900
categories: jekyll update
author: "developerjin"
excerpt: "Vue.js에서 데이터 갱신 후 UI조작시 생기는 문제와 해결방법을 알아봅시다."
---

### Vue.$nextTick

`Vue.js`를 어느정도 사용하고 있다가 데이터와 UI를 건드리는 상황에서 간헐적으로 DOM을 못찾는 상황을 겪어보셨을지 모르겠습니다. 겪지 않으셨더라도 언제가는 겪게 될 일이죠. (저는 이미 이 일을 겪게 되었습니다.)

네트워크 요청을 하던가, 어떤 데이터를 `vue._data`에 삽입하자마자 삽입 후에 갱신될 UI의 대해서 DOM을 탐색하여 처리할려고 하면 해당 DOM을 못찾아서 콘솔화면에 빨간색의 글자로 오류들을 내뿜게 됩니다.
이는 `vue._data`에 값들을 대입하게 되면 `Vue.js`에서 데이터갱신을 감지하고 UI를 갱신하게 되는데, 모든 데이터처리가 비동기로 처리되는 JavaScript의 특성때문에 UI가 갱신도 되기전에 DOM들을 탐색하는 상황이 나오게 됩니다.


```html
<!-- 오류 예제 -->
<div id="app">
	<div v-for="item in list">
    	<div v-bind:id="bindId(item)">{{item}}</div>	<!-- 2 -->
    </div>
</div>

<script>
new Vue({
	el: '#app',
    data: function() {
    	return {
        	list: []
        };
    },
    created: function() {
    	var self = this;

        for(var i=0; i<100; i++) {
        	this._data.list.push(i);	// 1
        }

        var dom = document.getElementById('item-0');	// 4
        dom.style.backgroundColor = 'red';				// 5
    },
    methods: {
		bindId: function(item) {
        	return 'item-' + item;	// 3
        }
    }
})
</script>
```
위와 같은 상황으로 작성하게 되면 오류가 나게 됩니다.
간단하게 설명을 드리면 `1`에서 list에 데이터들을 넣게 됩니다. Vue.js는 이를 감지하여 `2`, `3`을 통해 UI들을 갱신하게 되는데, UI를 갱신하기전에 `1`의 다음 라인인 `4`,`5`를 수행하게 됩니다. 아직 그리지도 않은 UI를 호출하려하니 당연히 오류가 나겠죠? Vue.js에서는 이런 일이 일어날 것을 예측하고, callback함수를 제공하고 있습니다.

```js
created: function() {

    // ...

    this.$nextTick(function() {
    	var dom = document.getElementById('item-0');
        dom.style.backgroundColor = 'red';
    });
}
```

`nextTick`으로 감싼뒤 callback을 통해 DOM을 조작하게 되면 `Vue.js`에서 데이터갱신 후 UI까지 완료한 뒤에 `nextTick`에 있는 함수를 최종적으로 수행하게 됩니다.

`nextTick`이 자주사용되기는 하는데 예제를 만들기가 애매한 부분이 있는 것 같습니다. 최대한 간단하게 설명하려다보니 위 코드대로 알려드리게 되었습니다. 더 좋은 의견이 있거나 좋은 예제가 있으면 언제든지 말씀해주시면 감사하겠습니다.

### 관련자료
- [Vue.nextTick 공식API문서](https://kr.vuejs.org/v2/api/#Vue-nextTick)
- [Vue.js 한국어 사용자 모임](https://vuejs-kr.github.io/)
- [jsfiddle 예제](https://jsfiddle.net/devjin0617/pgscu4q3/)
