---
title: github 블로그 만들기 (with hugo)
date: "2022-02-19T00:32:00.000Z"
template: "blog"
draft: false
slug: "/blog/hugo_tutorial/"
category: "etc"
description: "이전에 hugo를 이용해 블로그를 만들어보면서 기록해둔 아주 간단한 튜토리얼"
---
(해당 포스트는 2021년 08월 16일에 작성했습니다.)   
가방끈이 짧은 비전공자인 필자는 올해 초 이직을 성공했다.   
실무가 아닌 자잘한 업무를 진행함에도 불구하고 종종 난관에 부딪히곤 했다.   
구글링을 하다보면 당장은 문제를 해결 할 수 있지만, 다른 작업을 하다보면 금방 잊어먹게 되고 필요할 때 다시 찾아봐야 하는 악순환이 반복되었는데, 그런 고생을 덜어주기 위해 처음엔 Notion을 통해 링크 리스트를 만들어 필요시 다시 찾아보는 방법을 선택했었다.   
하지만 이 방법도 예전에 비해 조금 나아졌을 뿐 실력을 늘리는데 도움이 되지 못했다. 단순히 찾아보고 끝내지 말고 당일 저녁에라도 공부해보자며 이것저것 정보를 찾아보다 개발 블로그의 필요성을 느끼게 되었고 공부하는 과정과 결과물을 기록하고자 블로그를 제작하게 되었다.   


## Blog 만들기
※ 필자는 현재 Windows 사용 중이다.   
### Hugo 설치
일단 Hugo를 다운받고 설치해야 하는데,필자의 경우 압축파일 안에 있는 파일들을 C drive에 Hugo\bin 디렉토리를 생성한 후 bin폴더 안에 넣어주었다.   
2개의 Repository 생성   
Github action을 이용하면 하나의 Repository로도 사용할 수 있다고 한다. 하지만 그렇게 만들기 어려울 것 같아서 필자는 총 두개의 Repository를 만들었다.   
* 컨텐츠 및 소스 보관용 Repository 1 (blog 로 지었다.)   
* 실제로 구동 될 Repository 2 (아이디(혹은 팀 이름).github.io) 필자는 Repository 1의 경우 visibility를 ‘private’로 설정한 뒤 생성하였다. 이럴경우 나중에 푸시할 때 id & password 혹은 SSH key를 요구한다. (현재 개인용 컴퓨터로만 작성하기 때문에 SSH Key를 이용하는 중)


### hugo 새로운 사이트 생성
CMD나 PowreShell 혹은 git bash를 실행하고 Hugo를 설치한 디렉토리로 이동한 뒤
(필자는 C:\Hugo\bin에 위치) hugo new site <사이트명(임의지정가능)>을 입력하여 사이트를 생성했다.
```bash
/* git bash로 실행했을 때 */
$ cd c:
$ cd Hugo/bin
$ hugo new site blog
```
Hugo 테마에 가면 여러가지 테마들이 있다. 본인은 lubang님의 Hello Programmer를 fork 하여 로고 이미지 등을 수정한 뒤 사용하고 있다. 테마는 생성한 폴더, 즉 C:\Hugo\bin\blog\themes 폴더에 clone 후 테마 설명서에 따라 blog 최상위에 있는 config.toml 파일을 수정했다.
```bash
/* 현재 디렉토리가 Hugo/bin에 위치해있다면 blog로 이동 */
$ cd blog
/* 1번 선택지 : clone으로  git Repository 받아올 경우 */
$ git clone https://github.com/아이디/테마.git themes/테마이름
/* 2번 선택지 : submodule로 git Repository 받아올 경우 */
$ git submodule add https://github.com/아이디/테마.git themes/테마이름
```
### blog Remote, Submodule 설정 및 Github 연동
```bash
/* 현재 디렉토리 ->> C:/Hugo/bin/blog */
 
/* git 생성 */
$ git init
 
/* 'blog' Repository에 'blog' 사이트를 리모트 */
$ git remote add origin git@github.com:아이디/blog.git
 
/* submodule : github.io Repository를 blog Repository의 submodule로...
 hugo 명령어를 통해 public 폴더로 저장된 파일들을 .github.io.git Repository에 저장 */
$ git submodule add -b masater git@github.com:아이디/아이디.github.io.git public
```

…서브모듈 설정할 때 필자의 경우 아래와 같은 에러로 고생했다. public폴더는 생성되었지만 체크아웃이 되지 않았다.
```bash
fatal: 'origin/master' is not a commit and a branch 'master' cannot be created from it
Unable to checkout submodule 'public'
```
이런 상황에 대한 해답을 stackoverflow 에서 찾았다. 오류 무시하고 아래와 같이 명령어들을 입력하여 해결했다.
```bash
/* 현재 디렉토리 ->> C:/Hugo/bin/blog */
$ hugo
$ cd public
 
/* 현재 디렉토리 ->> C:/Hugo/bin/blog/public */
$ git add .
$ git commit -m "commit msg"
$ git push
```
```bash
/* 현재 디렉토리 ->> C:/Hugo/bin/blog/public */
$ cd ..
 
/* 현재 디렉토리 ->> C:/Hugo/bin/blog */
$ git add .
$ git commit -m "commit msg"
$ git push origin master
```
다시 상위 폴더로 이동한 뒤 커밋 앤 푸시를 진행하였다. 성공적으로 깃허브 연동까지 마무리되었다.
### 컨텐츠 생성, 테마 submodule 업데이트 —–> 업로드 (빌드)

C:\Hugo\bin\blog에 위치한 채로 아래와 같이 명령어를 입력하면
content폴더가 생성되며 명령어로 입력하여 생성 될 폴더 및 파일은 content 폴더 하위로 생성된다.
```bash
$ hugo new 폴더이름/파일이름.md
```
테마 리포지토리를 서브모듈 듈로 받아온 경우 서브모듈로 연결된 테마 레포지토리를 수정했을 때(이때 테마 레포지토리는 푸시까지 완료하여야 함) 아래 명령어로 편리하게 업데이트가 가능하다.
```bash
/* 현재 디렉토리 ->> C:/Hugo/bin/blog */
$ git submodule update --remote
```
파일 확장자를 보면 알 수 있듯이, 블로그 컨텐츠는 마크다운 파일을 이용하여 작성할 수 있다. 필자는 구글링 키워드 마크다운 문법을 통해 사용방법을 터득했다.   
컨텐츠 생성 및 테마 수정 후 저장하고 싶을 때 아래와 같이 커밋 앤 푸시를 하면 깃허브 페이지에 반영된다.
```bash
/* 현재 디렉토리 ->> C:/Hugo/bin/blog/public */
$ git add .
$ git commit -m "commit msg"
$ git push origin master
$ cd ..
/* 현재 디렉토리 C:/Hugo/bin/blog */
$ git add .
$ git commit -m "commit msg"
$ git push origin master
```

#### 참고 사이트
* Hugo 공식 : https://gohugo.io/getting-started/quick-start/
* lubang님 블로그 : https://blog.lulab.net/infra/install-hugo-and-configure-for-your-blog/
* integerous님 블로그 : https://ryan-han.com/post/dev/creating_static_blog/
* ialy님 블로그 : https://ialy1595.github.io/post/blog-construct-1/
* hoontaeklee님 블로그 : https://hoontaeklee.github.io/20191229_blogging_with_hugo/
* Stackoverflow : https://stackoverflow.com/questions/57867103/git-submodule-failure-when-building-pages-with-hugo