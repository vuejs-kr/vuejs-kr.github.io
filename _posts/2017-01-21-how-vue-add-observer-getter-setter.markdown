---
layout: post
title:  "Vue는 어떻게 data에 Observer / getter / setter 를 추가할까"
date:   2017-01-21 13:36:00 +0900
categories: jekyll update
author: "ChangJoo Park"
excerpt: "$data 의 observer와 getter, setter가 추가되는 과정을 알아봅니다."
---

## Vue 인스턴스 / 컴포넌트 데이터에 붙어있는 것들

Vue.js 를 이용하여 개발하는 도중 `console.log` 를 이용하여 데이터를 출력하는 경우가 있습니다. 이때 실제 코드로 작성한 데이터 코드에는 없는 객체/함수들을 볼 수 있습니다.

이번에는 Observer / get / set 이 어떤 과정을 거쳐 사용자가 작성한 Vue의 데이터 객체에 추가되는지 살펴보겠습니다.

## 예제에서 사용할 Vue 앱

이번에 작성할 Vue 앱은 매우 간단합니다.

HTML 과 JS 파일 전체 입니다.

```html
<!DOCTYPE html>
<html>
  <head>
    <script src="../../dist/vue.js"></script>
  </head>
  <body>
    <div id="app">
      {{message}}
    </div>
    <script>
      var app = new Vue({
        el: '#app',
        data: function () {
          return {
            message: 'hello'
          }
        }
      })
    </script>
  </body>
</html>


```

`data` 는 키가 **message**이고 값이 **hello**인 객체를 가지게 됩니다. 위 앱을 실행한 후 브라우저에서 확인합니다.

![Imgur](http://i.imgur.com/Jzr8ENw.png)

`app`은 위 코드로 만든 Vue.js 앱입니다. 그리고 app이 가진 데이터는 `app.$data`로 확인할 수 있습니다.

데이터는 **message** 가 있고 값도 **hello** 로 정확하게 들어가 있습니다. 하지만 `__ob__`과 `get`, `set` 을 발견할 수 있습니다. 

실제 Vue 코드에서 어떤 과정을 거쳐 이들을 추가하는지 살펴보겠습니다.



## Vue 인스턴스 생성 과정 중 initData 메소드 

결론부터 말하면 실제로 데이터를 처리하는 `initData` 메소드를 거쳐야 위 예제처럼 `__ob__`, `get`, `set` 이 추가됩니다.  `vue/src/core/instance/state.js` 경로의 `initData` 코드를 따라가봅니다.

```javascript
function initData(vm: Component) {
  let data = vm.$options.data
  data = vm._data = typeof data === 'function'
    ? data.call(vm)
    : data || {}
  if (!isPlainObject(data)) {
    data = {}
    process.env.NODE_ENV !== 'production' && warn(
      'data functions should return an object:\n' +
      'https://vuejs.org/v2/guide/components.html#data-Must-Be-a-Function',
      vm
    )
  }
  // proxy data on instance
  const keys = Object.keys(data)
  const props = vm.$options.props
  let i = keys.length
  while (i--) {
    if (props && hasOwn(props, keys[i])) {
      process.env.NODE_ENV !== 'production' && warn(
        `The data property "${keys[i]}" is already declared as a prop. ` +
        `Use prop default value instead.`,
        vm
      )
    } else {
      proxy(vm, keys[i])
    }
  }
  // observe data
  observe(data, true /* asRootData */)
}
```

`initData` 메소드의 전체 코드입니다. 중요한 부분은 Vue 인스턴스 혹은 컴포넌트의 `data`가 `function` 타입이 아닌 경우 처리하지 않습니다. 그리고 `props` 도 이 `initData`에서 처리합니다. 이번에는 data 에 대해서만 살펴보기 때문에 `props`에 대한 `proxy` 메소드는 지나갑니다. 맨 아래 줄의 `observe` 메소드가 호출되는 부분이 중요합니다.



## 데이터에 Observer 추가되는 과정

`observe`메소드는 `vue/src/core/observer/index.js`에 있습니다. 아래는 전체 코드입니다.

```javascript
/**
 * 값에 대한 observer 인스턴스를 만들도록 시도하고,
 * 새로 만든 경우 새 observer를 반환합니다.
 * observer가 존재하는 경우 기존의 observer를 반환합니다.
 */
export function observe(value: any, asRootData: ?boolean): Observer | void {
  if (!isObject(value)) {
    return
  }
  let ob: Observer | void
  if (hasOwn(value, '__ob__') && value.__ob__ instanceof Observer) {
    ob = value.__ob__
  } else if (
    observerState.shouldConvert &&
    !isServerRendering() &&
    (Array.isArray(value) || isPlainObject(value)) &&
    Object.isExtensible(value) &&
    !value._isVue
  ) {
    ob = new Observer(value)
  }
  if (asRootData && ob) {
    ob.vmCount++
  }
  return ob
}
```

Observer가 이미 존재하는 방법은 전달받은 `value`가 `Observer` 타입의 `__ob__` 를 가지고 있는지 확인하는 것 입니다. 없는 경우 새로 만듭니다. Observer 객체를 만드는 조건을 통과하면 새 `Observer` 객체를 만들 수 있습니다. 객체를 만들 때 `value`는 Vue 인스턴스의 `data` 입니다.

>`observerState` 객체는 `shouldConvert` , `isSettingProps` 를 가집니다. 기본값은 각각 true, false 입니다. 이 때문에 인스턴스를 만드는 상황에서 `observerState`는 기본값을 가지므로 `Observer` 객체를 만들 수 있게 됩니다.

## Observer 클래스 살펴보기

Observer 인스턴스는 `value` 를 이용하여 만듭니다. 

```javascript
// Observer 클래스 생성자
  constructor(value: any) {
    this.value = value
    this.dep = new Dep()
    this.vmCount = 0

    def(value, '__ob__', this)

    if (Array.isArray(value)) {
      const augment = hasProto ? protoAugment : copyAugment
      augment(value, arrayMethods, arrayKeys)
      this.observeArray(value)
    } else {
      this.walk(value)
    }
  }
```

살펴볼 부분은 `def(value, '__ob__', this)` 입니다. 익숙한 `__ob__` 가 보입니다. `def` 메소드의 코드 입니다. `def` 는 유틸리티 메소드 입니다.  `Object.defineProperty` 를 이용하여 `__ob__` 키를 추가합니다.

```javascript
/**
 * 속성을 정의합니다.
 */
export function def(obj: Object, key: string, val: any, enumerable?: boolean) {
  Object.defineProperty(obj, key, {
    value: val,
    enumerable: !!enumerable,
    writable: true,
    configurable: true
  })
}
```



이제 우리가 만든 Vue 앱의 data에 Observer가 추가 되었습니다. 이제 남은 것은 `get`과 `set` 입니다.

Observer는 전달받은 값이 배열인 경우 `observeArray`를 호출하여 각 배열의 아이템에 대해  `observe` 메소드를 실행합니다. 이번에 만든 Vue 인스턴스의 데이터는 `{ message: 'hello' }` 이고 `'hello'`는 문자열이므로 `walk` 메소드를  실행합니다.

```javascript
/**
 * 각 속성을 순회하여 getter/setter로 변환합니다.
 * 이 메소드는 타입이 Object인 경우에만 호출합니다.
 */
walk(obj: Object) {
  const keys = Object.keys(obj)
  for (let i = 0; i < keys.length; i++) {
    defineReactive(obj, keys[i], obj[keys[i]])
  }
}
```

이제 Vue 앱의 `data` 안에 있는 속성들을 순회하며 `defineReactive` 메소드를 실행하여 `getter/setter`를 추가합니다. 이를 통해 `console.log(app.$data)` 에서 나온 `get/set` 이 추가됩니다.

## getter와 setter를 추가하는 defineReactive 

`defineReactive` 전체 코드 입니다.

```javascript
/**
 * 객체에 반응형 속성을 정의합니다.
 */
export function defineReactive(
  obj: Object,
  key: string,
  val: any,
  customSetter?: Function
) {
  const dep = new Dep()
  const property = Object.getOwnPropertyDescriptor(obj, key)
  if (property && property.configurable === false) {
    return
  }

  // 사전 정의된 getter/setter 를 제공합니다.
  const getter = property && property.get
  const setter = property && property.set

  let childOb = observe(val)
  Object.defineProperty(obj, key, {
    enumerable: true,
    configurable: true,
    get: function reactiveGetter() {
      const value = getter ? getter.call(obj) : val
      if (Dep.target) {
        dep.depend()
        if (childOb) {
          childOb.dep.depend()
        }
        if (Array.isArray(value)) {
          dependArray(value)
        }
      }
      return value
    },
    set: function reactiveSetter(newVal) {
      const value = getter ? getter.call(obj) : val
      /* eslint-disable no-self-compare */
      if (newVal === value || (newVal !== newVal && value !== value)) {
        return
      }
      /* eslint-enable no-self-compare */
      if (process.env.NODE_ENV !== 'production' && customSetter) {
        customSetter()
      }
      if (setter) {
        setter.call(obj, newVal)
      } else {
        val = newVal
      }
      childOb = observe(newVal)
      dep.notify()
    }
  })
}
```

`defineReactive` 메소드는 꽤 깁니다. 하지만 단순하게 `def`에서 보았던 `Object.defineProperty` 이용하여 `get`과 `set` 속성을 추가합니다. `let childOb = observe(val)` 은 객체의 내부 속성까지 순회하면서 반응형 속성을 갖게 만들어 줍니다. 예제에서는 원시 문자열이므로 `undefined` 가 됩니다. 

이 과정을 거쳐 Vue 인스턴스의 data에 `Observer`를 추가되고 data 내부 속성에 `get/set`이 추가됩니다. `data`에는 이번에 사용한 원시값 이외에도 배열 / 객체 사용할 수 있습니다. 이 경우는 직접 코드를 살펴보시면서 이해하실 수 있을 것으로 생각하여 다루지 않습니다. 어떠한 경우라도 Vue는 data 안에 있는 내용에 대해 상황에 따라 순회하며 `observer` 메소드를 실행합니다.

## 전체 과정

Vue 의 코드를 github에서 다운받아 필요한 부분에 로그를 추가하고 눈으로 확인할 수 있습니다. 전체 과정에 대한 화면 입니다. 
빨간색 사각형 안의 내용을 따라가면서 보시면 됩니다.

![Imgur](http://i.imgur.com/vvY8LRl.png)

![Imgur](http://i.imgur.com/rAvSCqB.png)

![Imgur](http://i.imgur.com/dB28Sz7.png)

![Imgur](http://i.imgur.com/rVXFRoI.png)

![Imgur](http://i.imgur.com/VkgRy9U.png)

![Imgur](http://i.imgur.com/cgkt450.png)

![Imgur](http://i.imgur.com/tKSJooc.png)

![Imgur](http://i.imgur.com/QLy2o9n.png)

![Imgur](http://i.imgur.com/Jg33100.png)

감사합니다.

