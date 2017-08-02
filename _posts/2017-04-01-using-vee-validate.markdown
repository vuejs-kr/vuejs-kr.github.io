---
layout: post
title: "vee-validate 간단한 사용법 및 field-input 컴포넌트 소개"
date: 2017-04-01 12:00:00 +0900
categories: vue vee-validate
author: "gongzza"
excerpt: "vee-validate 기본 사용법 및 field-input 컴포넌트를 소개합니다."
---
[Vue.js][vue]에서 Form validator를 찾던 중 [VeeValidate][vv]를 알게 되었습니다.
[PHP Framework Laravel's validation syntax][lv#validation]에서 영감을 얻었다고 합니다. 마침 [Laravel][lv]로 작업중이여서 잘 되었다고 생각하고 사용하게 되었습니다.

## Install
``` bash
$ npm install --save-dev vee-validate
```

## Example
먼저 간단한 사용법을 보시겠습니다.
<script async src="//jsfiddle.net/gongzza/m67d8f4x/2/embed/result,html,js,css/dark/"></script>

1. Submit을 바로 눌러보면, `The email field is required.`와 동시에 빨갛게 표시되는 것을 볼 수 있습니다.
2. Email에 아무 값이나 입력을 하게 되면, `The email field must be a valid email.`라는 에러 메시지를 볼 수 있습니다.
3. 이메일 형식에 맞게 입력 후 Submit을 누르면, submit alert을 볼 수 있습니다.

### Validator
`Vue.use(VeeValidate)`을 사용할 경우  [vee-validate/mixin.js][vv#mixin.js]과 `v-validate` 디렉티브를 등록합니다.

mixin에서는 Validator를 생성해서 $validator로 대입하고 있습니다.
그리고 에러를 확인할 수 있는 errors를 computed로 등록하고 있습니다.

검증 순서는 다음과 같습니다.

1. input tag에 v-validate로 rules를 설정합니다.
2. this.$validator.validateAll()로 검증합니다.
3. this.errors.any()로 에러가 있는지 확인합니다.

특정 필드의 에러를 확인하고 싶은 경우 this.errors.has('field')로 에러가 있는지 확인 하고 this.errors.first('field')로 첫번째 메시지를 가져올 수 있습니다.

### Attributes
input tag에는 반듯이 name 또는 data-vv-name 속성이 있어야 하는데, 이 값은 필드명으로 사용됩니다.

필드명은 정의해서 출력할 수 있습니다.

- data-vv-as 속성 사용
- dictionary object 정의

먼저 data-vv-as를 설정할 경우

``` html
<input type="email" name="email" placeholder="Email" v-validate="'required|email'" v-model="email" data-vv-as="Email">
```

에러 메세지는 다음과 같이 바뀐 필드명으로 나오게 됩니다. `The Email field is required.`

하지만, 모든 필드에 data-vv-as 속성을 넣는 것은 여간 번거로운 일이 아닐 것입니다.

dictionary object를 이용해서 모든 email 필드명에 대해서 바뀐 이름으로 나오게 할 수 있습니다.

``` js
const config = {
  dictionary: {
    en: {
      attributes: {
        email: 'E-mail'
      }
    }
  }
}

Vue.use(VeeValidate, config)
```

그러면 에러 메시지는 이렇게 나오게 됩니다. `The E-mail field is required.`

### Localization
위에서 정의한 dictionary를 보면 en이 제일 먼저 정의 되어 있습니다. 즉, 다국어를 지원한다는 것인데, 다행히 한국어는 번역이 되어있는 상태였습니다.
([@ChangJoo-Park](https://github.com/ChangJoo-Park)님이 해주셧습니다.)

``` html
<script src="https://unpkg.com/vee-validate"></script>
<script src="https://unpkg.com/vee-validate/dist/locale/ko.js"></script>

<script>
Vue.use(VeeValidate, {
  locale: 'ko'
})
</script>
```

vee-validate를 먼저 호출하고, 다음에 locale javascript를 호출합니다.

Submit을 클릭하면, `email가 필요합니다.` 라는 한글로된 에러메시지를 볼 수 있습니다.

[Webpack][wp]을 사용할 경우 다음과 같이 설정할 수 있습니다.

``` js
import VeeValidate from 'vee-validate'
import ko from 'vee-validate/dist/locale/ko.js'

const config = {
  locale: 'ko',
  dictionary: {
    ko
  }
}

Vue.use(VeeValidate, config)
```

그리고 email을 이메일로 변경 하려면 다음과 같이 추가로 설정할 할 수 있습니다.

``` js
import { Validator } from 'vee-validate'
const dictionary = {
  ko: {
    attributes: {
      email: '이메일'
    }
  }
}

Validator.updateDictionary(dictionary)
```

### Messages
에러 메시지가 `이메일가 필요합니다.`, `이메일는 올바른 이메일 형식이어야합니다.` 처럼 조금 억양에 안맞게 출력되는 부분이 있습니다.
그래서 다음과 같이 변경할 수 있다.

``` js
import ko from 'vee-validate/dist/locale/ko.js'

ko.messages.email = (field) => `${field}은/는 올바른 이메일 형식이어야 합니다.`
ko.messages.required = (field) => `${field}이/가 필요합니다.`
```

또는 속성명에 조사를 붙이는 방법도 있을 것입니다.

``` js
import ko from 'vee-validate/dist/locale/ko.js'

ko.attributes.email = '이메일은'
ko.messages.required = (field) => `${field} 필수입력란 입니다.`
```

한글 번역본은 [ko.js][ko.js]에서 확인할 수 있다.

여기까지 간단한 사용법을 알아 보았습니다.

## field-input component
위에서 만든 예제로 폼을 구성하다보니 중복되는 코드가 너무 많아서 field-input 컴포넌트를 만들어 보았습니다.

[Demo](https://jsfiddle.net/gongzza/m67d8f4x/) `javascript란이 조금 길어서 링크로 연결하였습니다.`

field-input 컴포넌트를 이용해서 간단한 가입 폼을 만들어 보았는데요.
다음과 같이 룰이 적용 되어 있습니다.

- 이메일: `required|email`
- 이름: `required`
- 패스워드: `required|min:6`
- 패스워드 확인: `required|confirmed:password`
- 홈페이지: `url`

#### props
- value: v-model로 받기 위해서 사용
- field: scope.fieldName 형식으로 입력
- rules: 검사할 룰을 입력
- type: input tag의 타입을 입력, 기본값은 `text`입니다.

rules를 [String, Object]타입으로 받는 이유는 정규화표현식 파라메터 때문입니다.

[validator.js](https://github.com/logaretm/vee-validate/blob/d4d03c9980949b7f95c7b6102eb22fbf59cd5047/src/validator.js#L414)를 확인해보면 파라메터를 콤마(,)로 분리하는 것을 볼 수 있습니다.

예를 들어 `rules="regex:^\d{2,3}-\d{3,4}-\d{4}$"` 이렇게 넘긴다면, regex의 파라메터가 이렇게 배열로 `["^\d{2", "3}-\d{3", "4}-\d{4}$"]`넘어가서 에러가 발생합니다.

그래서 다음과 같이 Object 타입으로 파라메터를 넘길 수 있습니다.

``` js
v-bind:rules="{required: true, regex: [/^\d{2,3}-\d{3,4}-\d{4}$/]}"
```

Javascript로 처리하기 위해서 v-bind를 사용합니다.

#### computed
- prettyName: 프린트 되는 이름
- fieldName: 검사 받는 필드 이름
- scope: validator scope

prettyName은 [vue-i18n][vue-i18n]과 함께 사용하는 것이 좋은 것 같습니다.

이렇게 this.$t(this.field) 사용할 수 있습니다.

#### methods
- updateValue(val): 값을 검사하고 model을 업데이트 합니다.

getter와 context는 validator에서 검사하기위해서 값을 가져갈 때 호출됩니다.

그리고 mounted와 destroyed에서 검사할 field를 붙이고 제거합니다.

### Plugin
여기서 저는 Vue.use(VeeValidator)를 사용하지 않고 직접 플러그인을 만들어서 사용하였습니다.

이유는 VeeValidator로 추가하게 되면 모든 컴포넌트에 $validator를 매번 생성하기 때문입니다. [vee-validate/mixin.js][vv#mixin.js]

다음과 같이 validator를 하나 생성 후 모든 컴포넌트에서 하나의 인스턴스만 바라보도록 하였습니다.

``` js
import Vue from 'vue'
import {Validator} from 'vee-validate'

const VeeValidatePlugin = {
  install(Vue) {
    const validator = new Validator()
    Object.defineProperties(Vue.prototype, {
      $validator: {
        get() {
          return validator
        }
      }
    })

    // errors를 computed로 했을 경우
    // 캐싱되어서 그런지 에러 검사 후 표시가 되지 않음
    Vue.mixin({
      data() {
        return {
          errors: validator.errorBag
        }
      }
    })
  }
}

Vue.use(VeeValidatePlugin)
```
그리고 mixin으로 errors도 모두 같은 인스턴스를 바라보도록 하였습니다.
data를 사용한 이유는 computed가 캐시를 해서 그런지 `validateAll()`에 반응하지 않아서 였습니다.

## Conclusion
field-input 컴포넌트를 사용하니까 form이 매우 깔끔해진 것 같습니다. 물론 모든 input 타입에 대해서 확인해보진 않았지만, field-input 컴포넌트와 비슷하게 만들면 되지 않을까 생각됩니다.

하위 컴포넌트를 만드는 방법으로 [Event Bus ](http://vee-validate.logaretm.com/examples.html#event-bus-example)를 사용한 예제도 있습니다.

[wp]: https://webpack.js.org/
[lv]: https://laravel.com/
[lv#validation]: https://laravel.com/docs/master/validation
[vue]: http://kr.vuejs.org/
[vv]: http://vee-validate.logaretm.com/
[vv#mixin.js]: https://github.com/logaretm/vee-validate/blob/master/src/mixin.js
[ko.js]: https://github.com/logaretm/vee-validate/blob/master/locale/ko.js
[vue-i18n]: https://kazupon.github.io/vue-i18n/
