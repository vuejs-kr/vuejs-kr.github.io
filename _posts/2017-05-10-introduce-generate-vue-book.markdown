---
layout: post
title: "Vue.js 한국어 문서 전자책으로 만들기"
date: 2017-05-10 20:00:00 +0900
categories: vue epub
author: "ChangJoo Park"
excerpt: "EpubPress를 이용해 한국어 문서를 전자책으로 변환합니다"
---

이 문서는 Vue.js 공식 한글 문서를 epub 파일로 만드는 과정을 담고 있습니다.
한글 문서 관리에 많은 도움 부탁드립니다.
저장소는 https://github.com/vuejs-kr/kr.vuejs.org 입니다.

## EpubPress

[EpubPress](https://epub.press/)는 온라인 문서를 epub 또는 mobi 파일로 만들어주는 크롬/파이어폭스 확장 프로그램입니다. [김청진](http://jinblog.kr/)님께서 리디북스 페이퍼를 추천해주셔서 구입했는데 소설보다는 블로그 글이나 기술 문서를 epub으로 만들어서 보는데 사용하고 있습니다.

![](http://cfile10.uf.tistory.com/image/23014C3A590C69C1311547)
[리디북스 페이퍼(라이트) 소개글](http://jinblog.kr/190)

## EpubPress 설치하기

설치 방법은 크롬 브라우저를 열고 [EpubPress - Read the web offline](https://chrome.google.com/webstore/detail/epubpress-read-the-web-of/pnhdnpnnffpijjbnhnipkehhibchdeok?utm_source=chrome-ntp-icon)으로 들어가 설치하시면 됩니다.

![EpubPress 크롬 확장 이미지](http://i.imgur.com/0MAHggd.png)

### EpubPress로 Vue.js 공식문서를 책으로 만들기

책을 만드는 방법은 매우 간단합니다. 브라우저를 열고 필요한 탭을 모두 열고, EpubPress로 변환하기만 하면 됩니다.

저는 마이그레이션을 제외한 가이드의 모든 페이지를 탭으로 만들었습니다.

![탭 열린 상태](http://i.imgur.com/QlFkvVu.png)

열린 탭의 불러오기가 모두 끝나면 EpubPress 아이콘을 누릅니다.

![EpubPress 클릭](http://i.imgur.com/6qJxMME.png)

![EpubPress 선택](http://i.imgur.com/sLT1uzH.png)
제목과 설명을 간단히 적고 스크롤을 맨 아래로 보내 **Select All**을 누른 후 **Download**버튼을 눌러 변환을 시작합니다.


![변환중](http://i.imgur.com/ErRQ8Zf.png)

Download 버튼을 눌러 작업을 시작하면 일정 시간 후에 완료되었다는 메시지가 나오고 epub파일을 내려받습니다

![ibooks에서 열기](http://i.imgur.com/n5sGs1s.png)

맥에서는 iBooks로 볼 수 있습니다. 브라우저에서 보는 것이 훨씬 잘 나오지만 이동 중이나 기타 이유로 보아야할 필요가 있으면 유용하게 보실 수 있을 것으로 생각합니다.

vue-router, vuex, vue-loader 의 경우에는 gitbook을 사용하기 때문에 [gitbook-cli](https://github.com/GitbookIO/gitbook-cli)를 이용해 pdf,epub,mobi로 만드실 수 있습니다.

## 마무리

처음 epub press를 사용해서 변환한 것은 medium의 글이었는데 기술문서도 괜찮게 변환해주어서 소개하였습니다.

---
이 아래로는 광고입니다

vue.js 한국어 책입니다. 많이 읽어주시고 피드백 주셔서 감사합니다.

https://leanpub.com/vuejs2-korean
