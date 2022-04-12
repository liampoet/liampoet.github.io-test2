---
layout: post
title: jQuery 정규식
category: Blog
tags: [Git, Python, javascript, jQuery]
etc: 관리자페이지 제작 중, 앞부분이 동일한 id로 끝나는 엘리먼트들에 대한 접근을 jQuery로 처리해야하는 상황이 발생해서 여러 정보를 찾다가 jQuery의 id에 대한 정규식을 알게되었다.
---
## 💡 Intro
- FastAPI로 관리자페이지를 제작 중, 앞부분이 동일한 id로 끝나는 엘리먼트들에 대한 접근이 필요해졌다.
- [jQuery](https://jquery.com/)에 다양한 정규식이 있다는 것은 알고 있었지만, [id^=]라는 정규식은 이번에 처음 접하게 되었다. 사용법도 매우 간단해서 짧게 포스팅 해보려 한다. 
<br>
<br>
<br>

## 🔎 jQuery정규식이란?
---------------------------------------
***정규표현식***
- 특정한 규칙을 가진 문자열의 직합을 표현하는데 사용하는 형식 언어를 [정규표현식](https://www.nextree.co.kr/p4327/)이라고 한다.
- jQuery에도 다양한 search(), replace()등 다양한정규식이 존재하는데, 기초적인 예제는 아래와 같다.

***정규표현식 예제***

|기능|표현|
|:---:|:---:|
|모든 공백 체크|var regExp = /\s/g;|
|숫자만 체크|var regExp = /^[0-9]+$/;|
|이메일 체크|var regExp = /^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/i;|
|핸드폰번호 체크|var regExp = /^\d{3}-\d{3,4}-\d{4}$/;<br>var regExp = /^01([0|1|6|7|8|9]?)-?([0-9]{3,4})-?([0-9]{4})$/|
|일반 전화번호 체크|var regExp = /^\d{2,3}-\d{3,4}-\d{4}$/;|
|문자,숫자 4~20자|var regExp = /^[a-z0-9_]{4,20}$/;|
|휴대폰번호 체크|var regExp = /^01([0|1|6|7|8|9]?)-?([0-9]{3,4})-?([0-9]{4})$/;|

var re = /a/         --a 가 있는 문자열
var re = /a/i        --a 가 있는 문자열, 대소문자 구분 안함
var re = /apple/    -- apple가 있는 문자열
var re = /[a-z]/    -- a~z 사이의 모든 문자
var re = /[a-zA-Z0-9]/    -- a~z, A~Z 0~9 사이의 모든 문자
var re = /[a-z]|[0-9]/  -- a~z 혹은 0~9사이의 문자
var re = /a|b|c/   --  a 혹은 b 혹은 c인 문자
var re = /[^a-z]/  -- a~z까지의 문자가 아닌 문자("^" 부정)
var re = /^[a-z]/  -- 문자의 처음이 a~z로 시작되는 문장
var re = /[a-z]$/  -- 문자가 a~z로 끝남
var re = /s$/;          -- 공백체크
var re = /^ss*$/;   -- 공백문자 개행문자만 입력 거절
var re = /^[-!#$%& amp;'*+./0-9=?A-Z^_a-z{|}~]+@[-!#$%&'*+/0-9=?A-Z^_a-z{|}~]+.[-!#$%& amp;'*+./0-9=?A-Z^_a-z{|}~]+$/; --이메일 체크
var RegExpHG = "(/[ㄱ-ㅎ|ㅏ-ㅣ|가-힣]/)";  -- 한글 제거  
var RegExpJS = "<script[^>]*>(.*?)</script>";  -- 스크립트 제거  


![_config.yml]({{ site.baseurl }}/images/eventbridge검색.png)
<br>
<br>
<br>

## 📚 [id^=]사용하기

### id가 id_로 시작하는 엘리먼트들 접근

***Html***

<script src="https://gist.github.com/liampoet/e361f5c097a668e5045376d6c78e7e77.js"></script>

***jQuery***

<script src="https://gist.github.com/liampoet/d84a856ed2ecb8775d0ad690581e5a72.js"></script>

### id가 id_로 끝나는 엘리먼트들 접근

***Html***

<script src="https://gist.github.com/liampoet/10fa4177bb5c418bc9e8bac58a0f6560.js"></script>

***jQuery***

<script src="https://gist.github.com/liampoet/d46e913f0ffd5b46e0861544efccf4b0.js"></script>
<br>
<br>
<br>



**[참고자료]**
- [https://www.nextree.co.kr/p4327/](https://www.nextree.co.kr/p4327/)
- [https://dotheright.tistory.com/165](https://dotheright.tistory.com/165)
- [https://develop88.tistory.com/entry/Jquery-정규식-예제](https://develop88.tistory.com/entry/Jquery-정규식-예제)
