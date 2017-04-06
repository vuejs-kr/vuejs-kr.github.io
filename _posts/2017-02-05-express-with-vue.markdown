---
layout: post
title:  "express와 vue를 이용한 개발 환경 구성 살펴보기"
date:   2017-02-5 12:12:00 +0900
author: "ChangJoo Park"
excerpt: "express-generator와 vue-cli를 이용한 개발 전략을 사용합니다."
---

## 시작하기

이 글은 express 기반 서버 환경에서 vue를 시작하는 분을 위한 가이드 입니다. 여기서 설명하는 내용이 정답은 아닙니다. 실제로 저는 express를 사용해본 경험도 없습니다. 많은 분들께서 express를 사용하시고 있고, vue를 함께 사용하면 어떻게 해야하는지 고민하는 글을 보고 express를 시작하는 입장으로 작성하였습니다.

최종 결과물은 [https://github.com/ChangJoo-Park/express-generator-with-vue-cli](https://github.com/ChangJoo-Park/express-generator-with-vue-cli) 입니다.

### 이 글에서 다루는 내용

- 서버와 클라이언트 분리 전략
- express generator
- vue-cli
- webpack의 프록시테이블
- 배포
- vue-router의 해시모드
- vue-router의 히스토리 모드

### 이 글에서 다루지 않는 내용

- MongoDB등의 express와 함께 사용하는 데이터베이스 : 데이터베이스를 구축하지 않고 API 요청시 준비된 JSON을 반환합니다.

브라우저에서 URL이 요청되면 해당 URL에 따라 express 서버에 데이터를 요청하고 전달받은 데이터를 Vue를 이용해 출력합니다.



## 서버와 클라이언트 분리 전략

서버와 클라이언트 개발을 분리하는 것은 코드 관리에서 가장 중요하다고 생각하고 있습니다. 상황에 따라 다르지만 서로 다른 두영역이 한 코드베이스에 있을 필요가 없다고 여겨집니다.

Vue와 express 는 vue-cli와 express-generator라는 프로젝트 생성기를 제공하고 있습니다. Vue 개발에서 vue-cli를 사용하면 최초 프로젝트 구성에 시간이 적게 들어 개발에만 집중할 수 있도록 합니다. express-generator의 경우에도 express를 모르는 상황에서 고민할 필요없이 프로젝트 구성을 할 수 있도록 도와주었습니다. 

프론트엔드와 백엔드마다 제공되는 각 환경에 대한 도구를 합치는 일은 매우 힘들었습니다. 그리고 결과가 만족스럽지 않아 각 플랫폼에서 제공하는 도구를 사용하도록 하였습니다. 



## 프로젝트 기본 설정

이번 프로젝트의 기본 디렉터리 구조 입니다. 최상위 APP 디렉터리 아래에 express를 사용하는 backend, Vue를 사용하는 frontend 디렉터리가 있습니다.

_APP

 _backend

 _frontend



### express와 express-generator를 이용한 서버 개발

express의 [Express 애플리케이션 생성기  문서](http://expressjs.com/ko/starter/generator.html)를 따라 express를 설치하고 앱을 만듭니다.

```terminal
npm install express-generator -g #설치하지 않은 경우
express --view=jade backend
cd backend
npm install
DEBUG=backend:* npm start
```

http://localhost:3000에 접속하면 express 앱이 정상적으로 잘 작동하는 것을 확인할 수 있습니다.

이제 간단한 API를 만듭니다.

데이터베이스를 사용하지 않으므로 backend의 루트에 `movies.json`파일을 만들고 아래 내용을 추가합니다.

**`backend/movies.json`**

```json
[
  {
    "id": 1,
    "name": "공조",
    "year": 2017,
    "director": "김성훈",
    "poster": "http://img.cgv.co.kr/Movie/Thumbnail/Poster/000079/79416/79416_185.jpg"
  },
  {
    "id": 2,
    "name": "컨택트",
    "year": 2017,
    "director": "드니 빌뇌브",
    "poster": "http://img.cgv.co.kr/Movie/Thumbnail/Poster/000079/79437/79437_185.jpg"
  },
  {
    "id": 3,
    "name": "더킹",
    "year": 2017,
    "director": "한재림",
    "poster": "http://img.cgv.co.kr/Movie/Thumbnail/Poster/000079/79423/79423_185.jpg"
  },
  {
    "id": 4,
    "name": "모아나",
    "year": 2017,
    "director": "론 클레멘츠, 존 머스커",
    "poster": "http://img.cgv.co.kr/Movie/Thumbnail/Poster/000079/79316/79316_185.jpg"
  },
  {
    "id": 5,
    "name": "라이언",
    "year": 2017,
    "director": "가스 데이비스",
    "poster": "http://img.cgv.co.kr/Movie/Thumbnail/Poster/000079/79396/79396_185.jpg"
  },
  {
    "id": 6,
    "name": "너의 이름은",
    "year": 2017,
    "director": "신카이 마코토",
    "poster": "http://img.cgv.co.kr/Movie/Thumbnail/Poster/000079/79313/79313_1000.jpg"
  }
]
```



이제 위 정보를 가져올 수 있는 API를 만듭니다. 여기서는 두개를 만듭니다.

- 전체 영화 목록을 가져오는 API
- ID와 일치하는 영화를 가져오는 API

**`backend/routes/movies.js`** 

```javascript
var express = require('express');
var router = express.Router();
var movies = require('../movies.json');

router.get('/', function (req, res, next) {
  res.send(movies)
});

router.get('/:id', function (req, res, next) {
  var id = parseInt(req.params.id, 10)
  var movie = movies.filter(function (movie) {
    return movie.id === id
  });
  res.send(movie)
});

module.exports = router;
```

이 movies.js 파일을 사용할 수 있도록 `app.js`에 등록합니다.

```javascript
...
var index = require('./routes/index');
var movies = require('./routes/movies');
...
app.use('/', index);
app.use('/api/movies', movies);
```

이제 `http://localhost:3000/api/movies` 에 접속하면 영화 전체 목록이, `http://localhost:3000/api/movies/1`에 접속하면 영화 공조에 대한 내용을 가져올 수 있습니다.



### Vue.js와 vue-cli를 이용한 프론트엔드 개발

이제 Vue.js로 백엔드에서 작업한 내용을 출력할 차례입니다. 우선 vue-cli 작업을 합니다. 루트 APP 디렉터리에서 실행합니다.

```terminal
npm install -g vue-cli  #설치하지 않은 경우
vue init webpack frontend
#프롬프트마다 필요한 설정을 합니다. 
#vue-router 추가에 대한 질문은 꼭 Y를 해주세요
cd frontend
npm install
```

vue-cli의 템플릿인 `webpack`에 2017년 2월4일자로 vue-router가 추가 되었습니다. 많은 설정 없이 vue-router를 사용할 수 있게 되었습니다.

위 작업이 끝나면 express 서버와 연결하기 위한 설정을 해야합니다.

`frontend/config/index.js` 파일을 열어 `proxyTable`을 찾아 아래 내용을 추가합니다.

**`frontend/config/index.js`**

```javascript
proxyTable: {
  '/api': {
    target: 'http://localhost:3000/api',
    changeOrigin: true,
    pathRewrite: {
      '^/api': ''
    }
  }
},
```

위 설정을 하면 프론트엔드 개발 중 http://localhost:8080/api 요청이 왔을 경우 `http://localhost:3000/api`를 프록시로 사용하게 됩니다.  이 내용에 대한 자세한 설명은 [이 곳](http://vuejs-templates.github.io/webpack/proxy.html)을 읽어보세요

API 요청을 위해 axios도 추가합니다. `main.js`파일을 열어 아래 내용을 추가합니다.

**`frontend/src/main.js`**

```javascript
import axios from 'axios'
Vue.prototype.$http = axios
```

이제 Vue앱에서 `this.$http`로 HTTP 요청을 할 수 있습니다. axios에 대한 자세한 내용은 [이 곳](https://github.com/mzabriskie/axios)을 읽어보세요

Vue 앱의 페이지는 두개 입니다. 전체 영화 목록을 볼 수 있는 Index 페이지와 영화 하나의 내용을 볼 수 있는 Show 페이지 입니다. 이를 위해 두개의 단일 파일 컴포넌트가 필요합니다. 이름은 `IndexPage.vue`, `ShowPage.vue` 입니다.

**`frontend/src/components/IndexPage.vue`**

```html
<template>
  <div class="movies">
    <h1>영화 목록</h1>
    <div v-for="movie in movies" class="movie">
      <img v-bind:src="movie.poster" class="poster">
      <div>
        <strong>{{movie.name}}</strong>, <i>{{movie.director}}</i> [{{movie.year}}]
        <router-link :to="{ name: 'show', params: { id: movie.id }}">더보기</router-link>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  created () {
    this.$http.get('/api/movies')
    .then((response) => {
      this.movies = response.data
    })
  },
  data () {
    return {
      movies: []
    }
  }
}
</script>
```

별도의 스타일은 코드에 첨부하지 않았습니다.

**`frontend/src/components/ShowPage.vue`**

```html
<template>
  <div>
    <h1>상세 내용</h1>
    {{movie}}
  </div>
</template>

<script>
export default {
  created: function () {
    var id = this.$route.params.id
    this.$http.get(`/api/movies/${id}`)
    .then((response) => {
      this.movie = response.data
    })
  },
  data: function () {
    return {
      movie: {}
    }
  }
}
</script>
```

이 두파일을 vue-router에 연결합니다. `router/index.js`파일을 열어 수정합니다.

**`router/index.js`**

```javascript
import Vue from 'vue'
import Router from 'vue-router'

Vue.use(Router)

import Index from 'components/IndexPage'
import Show from 'components/ShowPage'
export default new Router({
//  mode: 'history',
  routes: [
    {
      path: '/',
      name: 'index',
      component: Index
    },
    {
      path: '/:id',
      name: 'show',
      component: Show
    }
  ]
})
```



라우터는 두가지 모드(해시/히스토리)를 사용할 수 있습니다. 지금은 해시 모드를 사용하기 때문에 `mode:history`를 주석처리했습니다. 모든 준비가 끝났으므로 `npm run dev` 명령어를 실행한 후 `http://localhost:8080`에 접속합니다.

![영화 목록 스크린샷](http://i.imgur.com/PPWVtT4.jpg)


데이터가 제대로 나오고 있지 않으면 express를 재시작해주세요. 현재 서버는 express와 vue의 webpack-dev-server 두가지가 작동중입니다. `http://localhost:3000`에 접속하면 express에서 제공하는 페이지가 나옵니다. 

이제 Vue 앱을 배포하여 express 서버만 사용해서 접속할 수 있도록 합니다.

frontend 디렉터리에서 `config/index.js` 파일을 열어 내용을 수정합니다.

**`config/index.js`**

```javascript
...
index: path.resolve(__dirname, '../../backend/public/index.html'),
assetsRoot: path.resolve(__dirname, '../../backend/public'),
...
```

`frontend`디렉터리에서 `npm run build` 명령어를 사용하면 `backend` 의 `public` 디렉터리에 Vue앱을 빌드한 결과를 만듭니다.

이제 express에서 할 일은 `backend/public`의 `index.html` 파일을 실행하는 것 입니다.

**`backend/routes/index.js`**

```javascript
var express = require('express');
var path = require('path');
var router = express.Router();

router.get('/', function (req, res, next) {
  res.sendFile(path.join(__dirname, '../public', 'index.html'))
});

module.exports = router;
```

`http://localhost:3000/`주소로 접속하면 `public/index.html`을 보여줍니다. 이 파일이 Vue앱입니다.

현재 Vue앱은 해시모드이기 때문에 `http://localhost:3000/`에 접속하면 영화목록 전체가 `http://localhost:3000/#/1`을 입력하면 영화 공조에 대한내용을 볼 수 있습니다. Vue 개발환경인 8080 포트와 동일하게 나옵니다.

이번에는 vue-router의 히스토리 모드를 사용해서 작업합니다. `frontend/routes/index.js`에서 주석처리한 `//  mode: 'history',` 의 주석을 제거합니다. 이번에도 `http://localhost:3000/`는 잘 작동합니다. 하지만 더보기를 클릭하면 없는 페이지라고 나옵니다. vue-router 가이드에 있는 [서버 설정 예제](https://router.vuejs.org/kr/essentials/history-mode.html)에 따라 [connect-history-api-fallback](https://github.com/bripkens/connect-history-api-fallback)를 추가합니다.

**`backend/app.js`**

```javascript
...
var app = express();
app.use(require('connect-history-api-fallback')())
...
```

이제 `http://localhost:3000/`과 `http://localhost:3000/1` 모두 정상적인 데이터를 출력합니다.



## 끝으로

express와 Vue를 이용한 개발을 해보았습니다. 각 라이브러리/프레임워크가 제공하는 도구를 최대한 활용하기 위해 백엔드와 프론트엔드를 분리하여 작업하였습니다. 더 많은 방법들이 있을 것으로 생각합니다. 이에 대한 토론은 [슬랙](https://t.co/vLwV3XXhck)/[페이스북](https://www.facebook.com/groups/1152461054807344/)/[깃허브](https://github.com/vuejs-kr/vuejs-kr.github.io/issues)에서 부탁드립니다.

