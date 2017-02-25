---
layout: post
title:  "크롬 브라우저에서 Vue 앱 디버깅하기"
date:   2017-02-25 17:53:00 +0900
categories: vue
author: "ChangJoo Park"
excerpt: "vue 컴포넌트를 크롬 개발자도구로 디버깅합니다"
---

# Vue.js 애플리케이션 디버깅



vue.js는 훌륭한 개발자 도구인 **vue-devtools**를 제공합니다. 현재 크롬 브라우저만 지원하고 있으며 [여기](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd)에서 받으실 수 있습니다.

![vue-devtools](https://raw.githubusercontent.com/vuejs/vue-devtools/master/media/demo.gif)



설치하신 후 vue.js 를 사용하는 웹페이지에 들어가면 **vue-devtools**를 사용할 수 있습니다

![vue-devtools](http://i.imgur.com/KWxhmSK.png)

vue-devtools를 이용해 컴포넌트간 관계와 데이터의 변화를 추적할 수 있지만 특정 메소드의 흐름을 깊이 추적할 수는 없습니다.

이번에는  _vue-cli의 webpack 템플릿을 사용하는 경우_  크롬 브라우저에서 vue 앱을 디버깅하는 방법을 알아봅니다.



```
vue init webpack my-vue-app
```

위 명령어와 같이 vue-cli를 이용해 앱을 만드는 경우 크롬 개발자도구에서 브레이크포인트를 걸어도 반응이 없습니다.



![break-point-not-working](http://i.imgur.com/AGnJl7P.png)



크롬을 이용해 vue 컴포넌트를 디버깅하기 위해서 _webpack_ 설정을 변경해야 합니다.

기본적으로 vue-cli의 webpack 템플릿은 _webpack.base.conf.js_를 webpack 셋팅의 기준이 되는 파일로 사용하고 개발환경과 배포환경별로 파일을 따로 제공합니다. 각각의 파일명은 _webpack.dev.conf.js_와  _webpack.prod.conf.js_ 입니다.

![setting](http://i.imgur.com/QhfZKVq.png)

수정해야하는 부분은 `devtool` 속성입니다. 값을 `'source-map'`으로 변경합니다. 그리고 서버를 재시작합니다

다시 크롬 브라우저에서 개발자도구의 `Sources`탭을 선택하고 디버깅할 **vue**파일을 엽니다. 스크립트 태그만 브레이크포인트를 걸 수 있습니다. 필요한 부분에 브레이크포인트를 걸고 UI에서 필요한 이벤트를 실행합니다.

![break-point-working](http://i.imgur.com/e5QriGU.png)

**참고자료**

- [웹팩 설정#devtool](https://webpack.github.io/docs/configuration.html#devtool)
- [vue-webpack-boilerplate](https://github.com/vuejs-templates/webpack)
