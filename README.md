<p align = "center">
<img src="https://vuejs.org/images/logo.png"/>
</p>
---

## Vue.js 한국어 사용자 모임입니다.

vue.js 관련 한국어 자료를 모으고 있습니다.

언제든지 올려주실 코드조각 또는 글이 있으시면 이슈에 [남겨주세요](https://github.com/vuejs-kr/vuejs-kr.github.io/issues/new)

## 로컬에서 `vuejs-kr.github.io` 서버 실행하는 방법

### 서버 실행하기전 설치요소

1. 요구 사항

    - Ruby v2.1.0 :arrow_up:
    - RubyDevKit

2. 설치

    ```bash
    $ gem clone vuejs-kr.github.io
    $ gem install bundler
    $ cd vuejs-kr.github.io
    $ bundler install # 의존성이 모두 설치됩니다.
    ```

3. 로컬 테스트 서버 시작

    ```
    $ bundler exec jekyll serve
    ```
