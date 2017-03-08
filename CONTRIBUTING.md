# 기여(CONTRIBUTING)

본 문서는 Vue.js 한국어 사용자 모임에 대한 기여 방법을 다룹니다. 2017년 2월 3일자 최신.

프로젝트에 기여 전에 저장소의 관리자와  [슬랙](https://t.co/vLwV3XXhck)/[페이스북](https://t.co/2GBbQ6z6Jv)/[메일](pcjpcj2@gmail.com)/[이슈](https://github.com/vuejs-kr/vuejs-kr.github.io/issues) 등의 방법을 통해 변경하려는 내용에 대해 먼저 논의하는 것을 추천합니다. 이는 이미 진행 중인 작업을 중복으로 처리하는 것을 막기 위함입니다.

## 행동강령(CODE OF CONDUCT)

Vue.js 한국어 사용자 모임의 행동강령은 따로 존재하지 않습니다.
단, 항상 상대방을 먼저 생각하는 마음으로 대해주시면 좋겠습니다.

## Github 사용자 모임 조직 멤버의 기준

[Github vuejs-kr 조직](https://github.com/vuejs-kr)의 멤버의 권한은 프로젝트 리뷰 및 머지까지 입니다. 누구나 함께할 수 있습니다.

멤버가 되는 기준은 현재(2017년 2월)까지의 기준은 다음과 같습니다.

- Vue.js 프로젝트에 기여한 분
- Vue.js 한국어 블로그에 포스트 및 코드조각을 올려주신 분

위 기준은 언제든지 변경 될 수 있으며 남을 비난하거나 강제할 권한은 없습니다.

## 프로젝트 범위

Vue.js 한국어 사용자 모임의 프로젝트 범위는 [Github vuejs-kr 저장소](https://github.com/vuejs-kr)를 기준으로 정합니다. 프로젝트별 기여 방법이 다릅니다. 이는 Vue.js 프로젝트를 관리하는 [메인 Vue.js 조직](http://github.com/vuejs)의 한계입니다. 문서에 관한 내용은 코어 Vue.js 조직에서 인지하고 있습니다.

- [**kr.vuejs.org**](https://github.com/vuejs-kr/kr.vuejs.org) : Vue.js의 한국어 공식 문서입니다.
- [**vue-router**](https://github.com/vuejs-kr/vue-router) : Vue.js의 컴패니언 라이브러리인  vue-router의 문서를 관리합니다. `docs/kr`에 문서가 있습니다.
- [**vuex**](https://github.com/vuejs-kr/vuex) : Vue.js의 컴패니언 라이브러리인 vuex의 문서를 관리합니다. `docs/kr`에 문서가 있습니다.
- [**vue-loader**](https://github.com/vuejs-kr/vue-loader) : 단일 파일 컴포넌트를 위한 vue-loader 입니다. `docs/kr`에 문서가 있습니다.
- [**vue-cli**](https://github.com/vuejs-kr/vue-cli) :Vue.js의 커맨드라인 인터페이스 입니다. 가이드를 위해 `README.md`만 관리합니다.
- [vuejs-kr.github.io](https://github.com/vuejs-kr/vuejs-kr.github.io) : Vue.js 한국어 사용자 모임의 공식 블로그 입니다.

## 각 프로젝트 별 기여 방법

#### kr.vuejs.org

Vue.js의 한글 가이드는 [영문 가이드](https://github.com/vuejs/vuejs.org)의 포크 프로젝트 입니다. 해당 프로젝트는 http://vuejs.org 의 최신 문서를 최대한 빠르게 반영하는 것을 목표로 합니다.

영문 공식 가이드의 내용이 추가되거나 삭제되는 부분은 현재 [박창주](https://github.com/ChangJoo-Park/)가 관리합니다.

한국어 가이드를 수정하기 위해서 다음 절차를 따라 진행하세요.

- 한국어 가이드 저장소를 포크
- 잘못 된 부분 수정 후 커밋
- 한국어 가이드 저장소에 Pull Request

##### 포크한 로컬 저장소와 한국어 가이드를 최신 버전으로 유지하는 방법

```
git remote add upstream https://github.com/vuejs-kr/kr.vuejs.org
git fetch upstream
git merge upstream/master
```

#### vue-router와 vuex, vue-loader 그리고 vue-cli

vue-router, vuex vue-loader는 gitbook을 이용하여 문서를 만듭니다. 이 문서들은 한국어 저장소에 있으나 vuejs 코어 저장소에서 수정이 되어야 실제로 반영이 됩니다. vue-cli을 제외한 저장소는 일본어 공식 조직의 [카즈야 카와구치(kazupon)](https://github.com/kazupon)의 리뷰를 거치게 됩니다. 한국어 문서의 경우 kazupon의 요청에 따라 [박창주](https://github.com/ChangJoo-Park/)가 리뷰를 합니다. 빠른 갱신을 위해 수정시 미리 알려주시면 리뷰 과정이 간단해집니다.

한국어 문서를 수정하기 위한 절차는 다음과 같습니다.

- 공식 조직의 해당 저장소를 포크하여 수정
- Pull Request
- 리뷰 후 머지

#### vuejs-kr.github.io 블로그

블로그에 대한 기여는 포스트/코드조각이 있습니다.

**포스트의 경우**
- Github 이슈 확인
- 이미 존재하는 이슈의 경우 할당 및 할당 요청 후 Pull Request
- 없는 이슈의 경우 생성 후 Pull Request
- 한국어 사용자 모임 Github 조직 멤버의 리뷰 후 머지
  Pull Request 할 포스트의 위치는 *_posts* 입니다.
  파일 이름은 `작성연도-월-일-제목.markdown`입니다. [현재 올라온 포스트들](https://github.com/vuejs-kr/vuejs-kr.github.io/tree/master/_posts)을 참조하신 후 요청해주세요.

**코드조각의 경우**
- Github 이슈 확인
- [현재 존재하는 코드조각 목록](https://vuejs-kr.github.io/snippets/) 확인
- [jsFiddle](http://jsfiddle.net/)에서 예제 작성 후 저장(로그인 필수)
- jsFiddle 링크를 [코드조각 페이지](https://github.com/vuejs-kr/vuejs-kr.github.io/blob/master/snippets.md)에 추가 후 Pull Request
- 한국어 사용자 모임 Github 조직 멤버의 리뷰 후 머지

## 기부

[이 링크](http://ko-fi.com/A316F4N)를 통해 기부를 받고 있습니다. 투명한 사용 내역 공개를 위해  ko-fi를 사용하고 있습니다. 기부금은 세미나가 열릴 경우에 사용합니다. 사용 시 모든 내역은 공개된 채널(슬랙/페이스북)에 공개합니다.

금전적인 기부 이외에 세미나 장소등에 대한 기부도 환영합니다.
