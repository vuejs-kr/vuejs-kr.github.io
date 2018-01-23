---
layout: post
title: "Vue Devtools 4.0 업데이트 안내"
date: 2018-01-24 12:00:00 +0900
categories: vue vue-devtools
author: "ChangJoo Park"
excerpt: "최근에 Vue.js용 개발자도구인 Vue Devtools가 업데이트 되었습니다. 어떤 기능이 추가되고 향상되었는지 살펴봅니다."
---

[What's new in Vue Devtools 4.0](https://medium.com/the-vue-point/whats-new-in-vue-devtools-4-0-9361e75e05d0)의 번역입니다.


최근에 Vue.js용 개발자도구인 Vue Devtools가 업데이트 되었습니다. 어떤 기능이 추가되고 향상되었는지 살펴보겠습니다.

## 컴포넌트 데이터 수정

이제 컴포넌트 인스펙터 화면에서 컴포넌트의 데이터를 수정할 수 있습니다.

1. 컴포넌트를 선택하세요.
2. `data` 섹션에 마우스를 올리세요.
3. 연필 아이콘을 클릭하세요.
4. 수정 후 완료 아이콘 또는 엔터를 누르세요. ESC 키를 누르면 취소됩니다.

<div style="position:relative;height:0;padding-bottom:75%"><iframe src="https://www.youtube.com/embed/xeBRtXLrQYA?ecver=2" style="position:absolute;width:100%;height:100%;left:0" width="480" height="360" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe></div>

필드의 내용은 시리얼라이즈된 JSON 값 입니다. 예를 들어, 문자열을 입력하려면 `"hello"`를 쌍따옴표와 함께 입력하세요. 배열이면 `[1, 2, "bar"]`일 것이고, 객체면 `{ "a": 1, "b": "foo" }` 처럼 보일 것입니다.

현재는 아래 타입의 값을 수정할 수 있습니다.

- `null` 그리고 `undefined`
- `String`
- 리터럴: `Boolean`, `Number`, `Infinity`, `-Infinity` 그리고 `NaN`
- 배열
- 기본 객체

배열과 객체의 경우 아이템을 제공되는 아이콘을 이용해 추가하거나 제거할 수 있습니다. 그리고 객체의 키 이름도 변경할 수 있습니다.

<div style="position:relative;height:0;padding-bottom:75%"><iframe src="https://www.youtube.com/embed/fx1zjvHryJ0?ecver=2" style="position:absolute;width:100%;height:100%;left:0" width="480" height="360" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe></div>

입력한 값이 정상적인 JSON 형식이 아니면 경고를 출력합니다. 그러나 `undefined`와 `NaN`은 직접 입력할 수 있습니다.

향후에 더 많은 타입을 지원할 예정입니다!

### Quick Edit

일부 값들은 'Quic Edit' 기능을 이용해 한번의 클릭으로 변경할 수 있습니다.

Boolean은 체크박스 아이콘으로 바로 토글할 수 있습니다.

<div style="position:relative;height:0;padding-bottom:75%"><iframe src="https://www.youtube.com/embed/llNJapRZaHo?ecver=2" style="position:absolute;width:100%;height:100%;left:0" width="480" height="360" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe></div>

숫자는 더하기 또는 빼기 아이콘으로 증감할 수 있습니다.

<div style="position:relative;height:0;padding-bottom:75%"><iframe src="https://www.youtube.com/embed/ZCToaOpId0w?ecver=2" style="position:absolute;width:100%;height:100%;left:0" width="480" height="360" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe></div>

## 에디터에서 컴포넌트 열기

vue-loader 또는 Nuxt 프로젝트를 하고 있다면, 선택한 컴포넌트를 사용하는 에디터에서 바로 열 수 있습니다 (싱글 파일 컴포넌트로)

1. 이 [설치 가이드](https://github.com/vuejs/vue-devtools/blob/master/docs/open-in-editor.md)를 따라하세요. (Nuxt 사용자라면 할 필요 없습니다.)
2. 컴포넌트 인스펙터에서 마우스를 컴포넌트 이름 위에 올리세요. 툴팁에 파일 경로가 나와야합니다.
3. 컴포넌트 이름을 클릭하면 에디터가 열립니다.

<div style="position:relative;height:0;padding-bottom:75%"><iframe src="https://www.youtube.com/embed/XBKStgyhY18?ecver=2" style="position:absolute;width:100%;height:100%;left:0" width="480" height="360" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe></div>

## 컴포넌트 원래 이름 보기

[manico](https://github.com/manico)의 Pull Request입니다.

기본적으로 모든 컴포넌트 이름은 CamelCase 입니다.  컴포넌트 탭의 '컴포넌트 이름 형식' 버튼을 눌러 토글할 수 있습니다.  이 설정은 저장되며, 이벤트 탭에도 적용됩니다.

<div style="position:relative;height:0;padding-bottom:56.14%"><iframe src="https://www.youtube.com/embed/PoZmEcCdSbU?ecver=2" style="position:absolute;width:100%;height:100%;left:0" width="641" height="360" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe></div>

## 쉽게 컴포넌트 검사하기

Vue Devtools가 열려있다면 컴포넌트 위에서 마우스 오른쪽 버튼을 클릭하면 바로 검사할 수 있습니다.

![Right-click a component in the page](https://cdn-images-1.medium.com/max/800/1*8fhP5VTb6uev-8HfI4stYw.png)

또한 `$inspect`라는 특수한 메소드를 이용해 프로그래밍으로 컴포넌트를 검사할 수 있습니다.

```vue
<template>
  <div>
    <button @click="inspect">Inspect me!</button>
  </div>
</template>

<script>
export default {
  methods: {
    inspect () {
      this.$inspect()
    }
  }
}
</script>
```

어떤 방법이든, 컴포넌트 트리가 새로 선택된 컴포넌트로 자동으로 확장됩니다.

## 컴포넌트 별 이벤트 필터

[eigan](https://github.com/eigan)의 Pull Request입니다.

이제 이벤트 히스토리를 컴포넌트별로 필터할 수 있습니다. `<`와 함께 컴포넌트 이름을 필터에 입력하면 됩니다.

<div style="position:relative;height:0;padding-bottom:56.14%"><iframe src="https://www.youtube.com/embed/wytquoUPSFo?ecver=2" style="position:absolute;width:100%;height:100%;left:0" width="641" height="360" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe></div>


## Vuex 인스펙터 필터

[bartlomieju](https://github.com/bartlomieju)의 Pull Request입니다.

이제 Vuex 인스펙터에서 필터가 가능합니다.

<div style="position:relative;height:0;padding-bottom:56.14%"><iframe src="https://www.youtube.com/embed/T095k5hI_pA?ecver=2" style="position:absolute;width:100%;height:100%;left:0" width="641" height="360" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe></div>

## 수직 레이아웃 지원

[crswll](https://github.com/crswll)의 Pull Request입니다.

개발자도구의 너비가 충분하지 않으면 버티컬 레이아웃으로 바꿀 수 있습니다. 수평일 때와 같이  위와 아래의 화면을 마우스로 크기 조절 할 수 있습니다.

<div style="position:relative;height:0;padding-bottom:75%"><iframe src="https://www.youtube.com/embed/33tJ_md8bX8?ecver=2" style="position:absolute;width:100%;height:100%;left:0" width="480" height="360" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe></div>

## 컴포넌트로 스크롤하기 - 향상됨

기본적으로 컴포넌트를 선택하면 화면이 더이상 스크롤되지 않습니다. 대신에 'Scroll into view' 아이콘을 클릭해야합니다

![Click on the eye icon to scroll to the compoent](https://cdn-images-1.medium.com/max/800/1*TJEfzB4ifK8t-5kpbZieRw.png)

이제 선택한 컴포넌트가 화면 가운데로 위치하게 됩니다.

## 인스펙터 접기

이제 여러 인스펙터의 섹션을 접을 수 있습니다. 키보드 단축키를 이용해 모두 접거나 한번 클릭으로 열 수 있습니다. 이는 Vuex 탭에서 변이의 세부사항만 보고싶은 경우 매우 유용합니다.

<div style="position:relative;height:0;padding-bottom:56.14%"><iframe src="https://www.youtube.com/embed/bblGueKPsjE?ecver=2" style="position:absolute;width:100%;height:100%;left:0" width="641" height="360" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe></div>

## 그 밖에

- 브라우저 환경에 기능이 없으면 'Inspect DOM' 버튼이 사라집니다.
- `-Infinity`를 지원합니다.
- 이벤트 훅 이슈가 수정되었습니다.
- 일부 코드를 정리했습니다.
- Date, RegExp, 컴포넌트 지원이 향상되었습니다. (그리고 해당 타입에서 time-traveling이 가능합니다)
- devtools는 [v-tooltip](https://github.com/Akryum/v-tooltip)을 사용합니다. (몇가지 이슈도 함께 수정했습니다.)

이미 확장프로그램을 사용하고 있으면 자동으로 `4.0.1`버전으로 업데이트 됩니다. [크롬](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd)과 [파이어폭스](https://addons.mozilla.org/fr/firefox/addon/vue-js-devtools/)에서 설치할 수 있습니다.

**업데이트를 가능하도록 도움을 준 모든 컨트리뷰터에게 고마움을 전합니다.**

문제가 있거나 새 기능을 제안하려면 [여기에](https://new-issue.vuejs.org/?repo=vuejs/vue-devtools) 알려주세요!

## 다음은?

새 릴리즈는 페이지에서 컴포넌트를 선택하는 것과 같은 더 많은 기능 (컬러피커)과 일부 UI 개선이 있을 것입니다.

또한 크롬과 파이어폭스뿐 아니라 모든 환경에서 디버깅할 수 있는 스탠드얼론 Vue devtools 앱, 새로운 라우팅 탭 및 설정 및 `Set`과 `Map`지원이 예정되어있습니다.
