---
layout: post
title: Vue.js 팀에 무엇이든 물어보세요
date: 2017-09-21 12:00:00 +0900
categories: vue
author: "ChangJoo Park"
excerpt: ""
---

2017년 09월 21일 새벽 2시에 있었던 AMA with Vue.js team 요약입니다.

원본 링크 : https://hashnode.com/ama/with-vuejs-team-cj7itlrki03ae62wuv2r2005s

수정사항 : VueConf 2018은 미국 뉴올리언스에서 열립니다

## React 라이센스 사태

React는 처음부터 BSD + patent clause를 유지했습니다. Vue는 특정 회사가 차지하고 있는 프로젝트가 아닙니다. Vue는 후원으로 유지되고 있습니다. 최악의 경우에도 MIT 정책을 유지할 예정입니다.

## Vue를 이용한 네이티브 모바일 앱 개발

Vue 코어팀이 직접 참여하는 네이티브 모바일(React Native 등)을 지원할 계획은 없습니다.
이미 **Weex**와 **NativeScript + Vue** 가 있습니다. Weex는 알리바바에서 지원하고 있습니다. NativeScript + Vue는 아직 성숙한 프로젝트가 아닙니다.

## 앞으로의 계획

- vue-test-utils (곧 베타 출시 예정)
- eslint-plugin-vue 3.0
- vue-cli complete overhaul
- Vue 3.0
- Vue 2.0 new features as well
- Official Style Guide
- Official Cookbook

추가로, 2.5 버전에는 유닛 테스팅 가이드가 추가되고, 타입스크립트 지원이 향상됩니다.

## 타입스크립트

타입스크립트를 충실히 지원할 것이나 메인 언어로 사용하지는 않을 것 입니다.

## Vue 3?

Vue 3를 이미 준비하고 있으며 API는 100%에 가깝게 유지할 것입니다. 주목할 것은 **Proxy** API를 적극적으로 사용하기 위해 구형 브라우저(IE 11 이하)를 지원하지 않을 것 입니다. **Proxy** API를 사용하게 되면 현재는 반응형으로 작동하지 않는 `this.myArray[1] = 'new_value'`와 같은 코드를 사용할 수 있습니다.


## Vue를 시작하려면?

공식 가이드를 읽는 것이 가장 좋습니다.

- "Introduction to Vue.js" video course on Frontend Masters, by Sarah Drasner
- "The Majesty of Vue.js" book, by Alex Kyriakidis (한국어 전자책/종이책이 있습니다.)
- "Vue JS 2 - The Complete Guide" Udemy course, by Maximilian Schwarzmüller


## 한 컴포넌트에 여러개의 루트 노드를 선언할 수 있습니까?

가독성이 떨어지기 떄문에 추천하지 않습니다.
하지만 아래와 같은 방식을 사용할 수 있습니다.
```
<template>
  <div v-if="loggedIn"></div>
  <div v-else></div>
</template>
```

혹은 함수형 컴포넌트를 사용하세요.

## Vue 릴리즈 이름은?

Evan You와 Egoist가 애니메이션 팬이라 릴리즈 이름을 정합니다.


## Evan You 이외의 코어팀의 역할은?

Evan You는 주로 Vue 코어를 담당합니다. 대부분의 코드는 Evan이 작성하지만 의사결정은 코어팀과 함께 합니다.

## Egoist와 Thorsten에 대하여

Egoist는 엄청나게 많은 Vue 관련 프로젝트를 만들고 있습니다. AI로 오해받을 정도입니다. 코어 팀에서도 한명이 아닌 개발 그룹이 아닌가 의심하고 있습니다.

Thorsten은 Vue Forum 또는 Github에서 엄청나게 많은 질문에 일일히 대답합니다. 코어팀은 Thorsten를 기계라고 부릅니다. 하지만 사람입니다 :)

## 개발도구는 어떤걸 사용합니까?

Atom, VSCode를 주로 많이 사용하며 코어팀 멤버 중 한명이  Emacs + Vim을 사용합니다.

## Nuxt와 같은 정적사이트생성기는 어떤게 있습니까?

Egoist의 [Ream](https://ream.js.org)은 Nuxt와 비슷하지만 사용법이 조금 더 유연합니다.
[Hexo](https://hexo.io/)는 공식 문서에서 사용하고 있습니다.

## 애플리케이션 상태의 Mutable 그리고 Immutable에 관한 생각은?

Mutable는 동일한 부분이 다른 코드에 의해 변형이 가능합니다. 이 경우 상태를 확신하기 어렵습니다. Vuex와 같은 엄격한 상태 관리 패턴을 사용하면 코드의 의도를 반영하는 방식으로 변경사항을 적용하기 때문에 시나리오에서 사용자를 보호합니다. Vuex가 변형이 가능한 스타일을 고수하는 이유는 Vue의 작동방식과 큰 연관이 있습니다. 의존성 추적은 상태가 완전히 새 값으로 대체되는 대신 변경할 때 가장 효율적입니다. Immutable.js 등을 사용하면 간접적인 다른 레이어가 생기고 immutable 래퍼와 원시 자바스크립트 값 사이의 추가적인 변환 비용이 생기므로 Vue에 적합하지 않다고 생각합니다.


## Vue 로고는 누가 만들었습니까?

에반유가 만들었습니다.
