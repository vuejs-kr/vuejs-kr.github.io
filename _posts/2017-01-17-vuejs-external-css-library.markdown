---
layout: post
title:  "Vue.js 외부 CSS 라이브러리 추가하기"
date:   2017-01-17 15:35:31 +0900
categories: jekyll update
author: "Lee Sun-Hyoup"
excerpt: "Vue.js에서 SCSS, LESS, PostCSS와 같은 외부 라이브러리를 추가하는 방법에 대해서 알아봅시다."
---

Vue.js는 `vue-loader`를 통해서 다른 Webpack 로더를 불러와서 Vue 컴포넌트를 처리 할 수 있습니다. 그 방법을 통해서 유용하게 쓰이는 것 중 하나가 CSS를 처리하는 방법인데요, 이번 포스팅에서는 **SCSS, LESS, Stylus**와 같은 전처리기와 **PostCSS**와 같은 후처리기를 Vue.js에서 불러오는 방법에 대해서 알아보겠습니다.

## 전처리기(Pre-Processer)
CSS에서 **전처리기**란 이름 그대로 CSS를 **먼저 처리하는** 도구입니다. CSS보다 더 간단하고 구조적으로 작성할 수 있는 문법을 제공하고 그 문법에 맞춰서 스타일을 작성하면 자동으로 CSS 파일을 만들어줍니다. 이러한 전처리기에는 **SCSS, LESS, Stylus** 등이 있습니다. 간단한 예제 코드를 통해 차이점을 알아봅시다. 이 예제는 SCSS를 통해 작성되었습니다.

### CSS
```css
body {
	font-size: 12pt;
	color: #012345;
}

body a {
	color: #555555;
}
```

### LESS
```scss
$body-color: #012345;
$anchor-color: #555555;
body {
	font-size: 12pt;
	color: $body-color;

	a {
		color: $anchor-color;
	}
}
```

보시다시피 전처리기는 **변수**와 **중첩** 문법을 사용할 수 있습니다.

## 후처리기(Post-Processor)
**후처리기**는 마찬가지로 이름 처럼 CSS를 작성한 **후에 처리하는** 도구입니다. 주로 후처리기는 `PostCSS`라는 것을 사용합니다. `PostCSS`의 주요한 플러그인 중 하나인 Autoprefixer에 대해서 소개해드리겠습니다. 흔히 CSS를 작성할 때에는 크로스 브라우징을 염두해두고 작성합니다.

```css
.flexbox {
	display: -webkit-box;
	display: -webkit-flex;
	display: -ms-flexbox;
	display: flex;
}
```

이러한 작업은 SASS, LESS와 같은 전처리기가 등장하면서 조금 더 편해졌습니다.

```scss
@mixin flexbox() {
	display: -webkit-box;
	display: -webkit-flex;
	display: -ms-flexbox;
	display: flex;
}

.flexbox {
	@include flexbox;
}
```

위 처럼 미리 Mixin을 만들어 둔 후 include하는 방식을 통해 중복된 작업을 줄일 수 있었습니다. 이렇게 작성하면 기존 CSS를 작성하는 것보다 간단하지만 두 가지 신경써야할 점이 있습니다. 첫 번째는 브라우저의 지원 상황을 개발자가 알고 있어야합나다. 두 번째는 개발자의 실수로 빼먹을 수 있다는 점입니다. 만약 후처리기를 사용한다면 이런 문제를 쉽게 해결 할 수 있습니다.

```css
/* input.css */
.flexbox {
	display: flex;
}

/* output.css */
.flexbox {
	display: -webkit-box;
	display: -webkit-flex;
	display: -ms-flexbox;
	display: flex;
}
```

보시다시피 후처리기를 사용하면 별다른 작업없이 알아서 vender prefix를 더해줍니다.

## `vue-loader`를 통해 가져오기
만약 CSS 전처리기와 후처리기를 사용한다면 개발자의 생산성을 대폭 증가시킬 수 있을겁니다. 이제 우리는 Vue 컴포넌트에도 이 도구들을 적용시키고 싶습니다. 어떻게 해야할까요? 생각보다 매우 간단합니다. 여기서는 `vue-cli`를 통해 시작하겠습니다.

### 전처리기 추가하기
먼저 전처리기를 추가해봅시다. Vue에서 전처리기를 사용하는 방법은 매우 간단합니다. `*.vue` 파일의 스타일 블럭에 `lang='scss'` 속성을 추가헤주세요. 그 후에 노드 서버를 실행시켜봅시다.

```
ERROR in ./src/App.vue
Module not found: Error: Cannot resolve module 'sass-loader' in /path/src
 @ ./src/App.vue 5:0-170
```

그렇다면 위와 같은 에러를 볼 수 있습니다. 전처리기를 사용하기 위해서는 적절한 Webpack 로더를 설치해야합니다. 여기서는 SCSS를 사용하기 위해 `sass-loader`와 `node-sass`를 설치하겠습니다.

```bash
$ npm install sass-loader node-sass --save-dev
```

설치가 완료된 후 새로고침을 통해 확인해보면 (혹은 핫 리로딩에 의해) 적용이 완료된 화면을 볼 수 있습니다!

### 후처리기 추가하기
후처리기를 추가하는 방법은 전처리기를 추가하는 방법보다 아주 조금 더 복잡합니다. 왜냐하면 Webpack 설정을 수정해야하기 때문입니다. 먼저 Webpack 설정을 확인해봅시다. 만약 `vue-cli`로 `webpack` 템플릿을 불러왔을 경우 build 디렉토리에 `webpack.base.conf.js` 파일을 확인해봅시다.

```js
vue: {
  loaders: utils.cssLoaders({ sourceMap: useCssSourceMap }),
  postcss: [ // 이 리스트에 추가하면 됩니다.
    require('autoprefixer')({
      browsers: ['last 2 versions'] 
    })
  ]
}
```

사실 `vue-cli`를 이용해서 `webpack` 템플릿을 불러오면 이미 `autoprefixer` 플러그인이 붙어있습니다. 다음 [링크](https://vue-loader.vuejs.org/kr/features/postcss.html)를 확인하시면 PostCSS 플러그인을 불러오는 방법에 대해서 더 자세히 알 수 있습니다.

## 마지막으로...
`vue-loader`를 이용하면 Vue를 위한 모듈 중 CSS 뿐만 아닌 더 다양한 모듈을 불러올 수 있습니다! 예를 들어 `CoffeeScript`나 `TypeScript`를 사용한다던가 `Linting` 등을 할 수도 있습니다. 혹은 `e2e Test`, `Unit Test`도 가능합니다. 더 좋은 개발 환경을 위하여 `vue-loader`에 대해서 알고 싶다면 Reference 항목의 링크를 참고해주세요!

## Reference
* [vue-loader 문서](https://vue-loader.vuejs.org)
