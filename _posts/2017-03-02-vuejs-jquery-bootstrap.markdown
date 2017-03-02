---
layout: post
title:  "vue.js에서 jQuery, Bootstrap 추가하여 사용하기"
date:   2017-03-02 18:38:00 +0900
categories: jekyll update
author: "developerjin"
excerpt: "가장 많이 사용하는 jQuery와 Bootstrap을 추가하여 사용해봅시다."
---

작성자 블로그URL: [http://blog.puding.kr/180](http://blog.puding.kr/180) 

국내에서도 `Vue.js`를 사용하는 비율이 점점 높아져가는 것 같습니다. Angularjs, Reactjs보다 진입장벽이 낮고, HTML/CSS와 기본 JavaScript만 다룰줄 알더라도 쉽게 사용할 수 있다는 장점이 있습니다. 또한 `vue-cli`라는 툴을 이용하여 기본적인 vue 골격을 만들어 웹팩(webpack)까지 통합세트로 운영할 수 있다보니, 고민없이 사용해야한다라는 생각이 점점 강해지는 것 같습니다.

분명 처음에는 가볍게 시작을 하면서, 분명 사용하기도 편합니다. 그런데 이 전에 사용했었던 jQuery, Bootstrap. 그리고 그 외의 많은 라이브러리들을 어떻게 추가해야하는지 헷갈릴 수도 있을 것 같습니다. 이는 웹팩이 같이 포함되어 있으면서 외부 라이브러리들을 어떻게 추가하는게 맞는 것인지 명확하게 나와있는 곳 또는 설명해주는 곳이 없기 때문이라고 생각됩니다.

저 역시도 jQuery를 추가할 때 webpack config에 끼워넣을려고 고민을 엄청했고, 여러가지 방법을 찾았습니다. 그 중에서 가장 쉬우면서 깔끔하게 추가하는 방법을 포스팅에 정리해보려 합니다.



외부 라이브러리를 추가하기 전에 `vue-cli`를 이용하여 프로젝트 하나를 생성합니다. 이 때, `webpack-simple`보다는 `webpack`으로 생성하시는 걸 추천합니다. (나중 웹팩을 추가적으로 만져야할 때 편리합니다.)

## jQuery

jQuery가 뭐하는 건지 잘 모르시는 분 들을 위해 [생활코딩](https://opentutorials.org/course/53/45) 강좌사이트에 나와있는 내용을 언급하도록 하겠습니다.

> jQuery란?
>
> 1. 엘리먼트를 선택하는 강력한 방법과
> 2. 선택된 엘리먼트들을 효율적으로 제어할 수 있는 다양한 수단을 제공하는
> 3. 자바스크립트 라이브러리

네. 그렇다고 합니다.

HTML을 마음대로 조작하며, 기존 JavaScript문법을 좀 더 효율적으로 사용할 수 있도록 도와주는 외부 라이브러리 입니다.

기존 웹사이트에서 jQuery를 추가하려면 HTML에 스크립트 파일을 호출하여 사용하면 됩니다.

```html
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.1.1/jquery.min.js"></script>
```

바로 이런식으로 말이죠.



하지만 Vue에서 위 방식으로 하면 뭔가 하지말아야 할 행동을 하는 것 같기도 하고, 뒤가 찝찝하기도 하고, 뭐라 말할 수 없는 느낌이 있습니다. (위 방식대로 사용해도 문제는 없습니다.)



jQuery를 추가하려면 일단 외부 저장소에서 jQuery 라이브러리를 가져와야겠죠? 해당 프로젝트 루트에서 다음과 같은 명령어를 실행합니다.

```bash
npm i --save-dev expose-loader
npm i --save jquery
```

라이브러리를 설치하면서 `package.json`에 같이 추가하게 됩니다.

이 것으로 모든 준비는 끝났습니다. 코드 몇줄로 추가하고 그냥 사용하면 됩니다.

`vue-cli`로 프로젝트를 생성했다면 `/project/src/main.js`파일을 확인하실 수 있습니다. 해당 파일에 jQuery사용을 위해 한줄 코드를 추가합니다.

```javascript
import 'expose-loader?$!expose-loader?jQuery!jquery'
```

위 코드를 추가하고, `npm run dev`로 서버를 실행하고 `http://localhost:4000/`으로 접속하세요. 그리고 개발자도구에서 `$`를 사용해보세요! 이 것으로 jQuery를 사용할 수 있게 되었습니다.



## Bootstrap

반응형 웹사이트를 개발하기 위해 트위터에서 오픈소스로 공개한 `UI Framework`입니다. jQuery기반으로 되어 있으며, PC, Tablet, Mobile의 스크린을 자유롭게 설계하며 UI를 표현할 수 있는 프레임워크입니다.

`Bootstrap`의 경우는 javascript코드 이외의 UI를 표현해주는 `css`파일도 포함시켜줘야 합니다. 그렇다하더라도 코드는 2줄로 추가할 수 있습니다.

먼저 외부저장소에서 Bootstrap을 가져옵니다.

```shell
npm i --save bootstrap
```

그리고 위에서 수정했던 `/project/src/main.js`을 다시 엽니다.

```javascript
import 'expose-loader?$!expose-loader?jQuery!jquery'
// 위에서 추가했던 jQuery 밑에 코드를 작성하세요

import 'bootstrap'
import 'bootstrap/dist/css/bootstrap.min.css'
```

지금부터 `jQuery`와 `Bootstrap`을 동시에 사용할 수 있게 되었습니다.

`Bootstrap`의 `.js`, `.css`가 잘 추가되었는지 테스트해보려면 HTML코드작성하는 부분에 아래와 같이 추가해보세요.

```html
<template>
...
  <button type="button" class="btn btn-primary" autocomplate="off" data-loading-text="jquery with bootstrap" @click="clickBtn">
...
</template>
  
<script>
  export default {
    name: 'jQueryApp',
	methods: {
      clickBtn (event) {
        $(event.target).button('loading')
        
        setTimeout(function() {
          $(event.target).button('reset')
        }, 1000);
      }
	}
  }
</script>
```

위 코드가 정상적으로 작동한다면 jQuery, Bootstrap(js, css)가 Vue에서 정상적으로 작동하는 것을 확인하실 수 있을 것입니다.

jQuery를 추가하지 못해서 망설였던 분들을 위 방식으로 쉽게 시작하실 수 있으며, jQuery, Bootstrap 뿐만 아니라 그 외의 자주 사용했었던 라이브러리들도 이러한 형식으로 쉽게 추가할 수 있습니다.

Front-end를 개발하고 있음에도 아직 Framework를 사용하고 계시지 않고, 부담스러워하시는 분들이 있다면 지금 당장 시작해보세요. 하루만 투자하면 원하는 웹 애플리케이션을 개발하실 수 있습니다 :smile:



### 관련 정보

- Vue.js 공식홈페이지 [영문](https://vuejs.org/) [한국어](https://kr.vuejs.org/)
- Vue.js 한국어 사용자 모임 [링크](https://vuejs-kr.github.io/)
- vue-cli [링크](https://github.com/vuejs/vue-cli)
- 생활코딩 - jQuery란? [링크](https://opentutorials.org/course/53/45)
- jQuery [링크](https://jquery.com/)
- Bootstrap [링크](http://getbootstrap.com/)
- expose-loader [링크](https://github.com/webpack-contrib/expose-loader)

