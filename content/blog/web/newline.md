---
title: 오라클에서 불러온 텍스트 데이터 HTML(Thymeleaf)에서의 개행 처리
date: "2022-03-01T21:55:00.000Z"
template: "blog"
draft: false
slug: "/blog/newline/"
category: "web"
description: "어제 업무하면서 알게된 것을 정리하였다."
---
## 오라클에서 개행문자 처리
오라클에서 개행이 포함된 텍스트 데이터를 HTML에서 개행 처리하여 출력하고자 할 때 개행문자를 <br/>로 변경해주면 아주 간단하게 처리해줄 수 있다.   
필자가 처음 했던 방법은 이러하다.
```java
// String 형식
//...
txtData.replaceAll("\n", "<br/>");
```
```html
<div>
    "abcde "
    <br>
    "fghij dd"
</div>
```
예를 들어 abcde와 fghij dd 사이에 개행처리한 텍스트 데이터를 불러와 자바에서 replace처리 한다고 가정하자.
위의 html코드는 크롬 개발자모드에서 확인한 html 코드다.   
브라우저 상에서는 abcde와 fghij dd가 개행처리가 잘 되었음을 확인하였으나 실제 개발자모드에서
확인해보면 개행처리 하기 전 문장에 공백이 그대로 남아있음을 확인하였다.   
그냥 두기엔 왠지 찝찝하여 구글링 하는 도중 아래 링크에서 해답을 찾았다.   
<https://baessi.tistory.com/84>   
<https://itlove.tistory.com/264>
```java
// String 형식
//...
txtData.replaceAll("\r\n", "<br/>");
```
```html
<div>
    "abcde"
    <br>
    "fghij dd"
</div>
```
위의 방법대로 다시 해보니 정상적으로 출력되는 것을 확인할 수 있었다.
오라클에서의 개행문자는 \n으로만 표현될 줄 알았는데 \r\n이 모두 포함되는 거였다. DB에서의 개행문자에 대해선 너무 아는것이 없어 좀 더 공부해본 뒤 추가로 기록해둘 예정이다.   
## Thymeleaf에서 html 태그를 포함한 변수(텍스트) 출력 시
thymleaf를 접한지 몇일 되지 않아서 꽤 난감한 상황이었고 다시 이런 실수를 하지 않고자 작성하게 되었다.
```html
<div th:text='${txtData}'>
```
위의 `th:text`는 일반적으로 텍스트를 출력할 때 사용한다. 만일 html를 포함할 시 브라우저에
 html태그가 텍스트 형태로 나오게 된다.
```text
    abcde <br> fghij dd
```
만일 html태그가 있고, 그 태그를 적용시키려면 `th:utext`를 사용하여야 한다.
```html
<div th:utext='${txtData}'>
```
```text
    abcde
    fghij dd
```
위의 방법대로 코드를 입력하면 브라우저에서 정상적으로 작동되는 것을 확인할 수 있다.
   
   
### 참고 사이트
<https://baessi.tistory.com/84>   
<https://itlove.tistory.com/264>
<https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html>