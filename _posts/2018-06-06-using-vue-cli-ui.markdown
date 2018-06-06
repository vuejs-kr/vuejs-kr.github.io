---
layout: post
title: "Vue CLI 3 UI 사용하기"
date: 2018-06-06 8:00:00 +0900
categories: vue vue-cli vue-cli-ui
author: "ChangJoo Park"
excerpt: "Vue CLI를 UI로 조작할 수 있는 Vue CLI UI를 소개합니다."
---

# Vue CLI UI 사용하기

Vue CLI 3의 베타 15가 릴리즈되었습니다. 이번 버전의 가장 큰 변경사항은 Vue CLI의 UI버전이 공식적으로 포함된 것입니다.

Vue CLI UI를 이용하면 Vue CLI의 기능을 UI에서 사용할 수 있습니다.

![Vue CLI UI](https://i.imgur.com/MF0iOtF.png) 

## 설치

UI를 사용하려면 우선 Vue CLI 3의 beta 15버전이 설치되어 있어야 합니다. NPM 또는  yarn을 이용할 수 있습니다.

```bash
npm install -g @vue/cli
# 또는
yarn global add @vue/cli
```

Vue CLI 3가 설치되면 터미널에서 `vue` 명령어를 이용해 Vue.js 관련 프로젝트를 만들고, 여러 태스크를 이용해 개발할 수 있습니다.

## Vue CLI UI 사용하기

Vue CLI UI는 공식적으로 브라우저 환경에서 작동하도록 되어있습니다. 아래 명령어로 Vue CLI UI를 실행하세요.

```bash
vue ui
```

맥 OS를 사용하고 있고 독립적인 앱으로 사용하려는 경우 [이 저장소](https://github.com/egoist/vue-cli-gui)의 Release 탭에서 최신버전을 이용해주세요.


Vue CLI는 많은 기능을 가지고 있고 직접 사용해보시는 것이 가장 좋습니다.
아래의 작업을 할 수 있습니다.

- 프로젝트 만들기
- 프로젝트에 플러그인 추가/제거 및 추가 설정
- ESLint 설정 (Vue 관련 설정 포함)
- 각 NPM 태스크 관리

## 한글화

Vue CLI UI는 운영체제에 따라 자동으로 로케일 파일을 불러옵니다. 한국어 운영체제를 사용하고 계시면 자동으로 한글로 보실 수 있습니다. Vue CLI UI의 한글 로케일은 [이곳](https://github.com/vuejs-kr/vue-cli-locale-ko)에서 관리하고 있습니다. 사용 중 오타나 오역 등을 Pull Request 해주세요.
