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

## 🌩 jQuery정규식이란?
---------------------------------------
***정규표현식***
- 특정한 규칙을 가진 문자열의 직합을 표현하는데 사용하는 형식 언어를 [정규표현식](https://www.nextree.co.kr/p4327/)이라고 한다.
- 

![_config.yml]({{ site.baseurl }}/images/eventbridge검색.png)

Amazon EventBridge에서 이벤트목록에 규칙을 들어가면 규칙생성이 있는데, 다른 부분 건들것 없이 그냥 규칙생성만 클릭해주면 된다. 

![_config.yml]({{ site.baseurl }}/images/규칙생성.png)
<br>
<br>

### 2. 규칙 세부 정보 설정하기

***1단계 - 규칙 세부 정보 정의***

원하는 규칙이름을 정하고, 나는 저번 포스트에 공부하였던 Cron식을 사용할 것이기 때문에, 규칙 유형을 일정으로 하고 다음버튼을 클릭한다.

![_config.yml]({{ site.baseurl }}/images/규칙1.png)
<br>
<br>

***2단계 - 일정 정의***

규칙2에서는 패턴을 설정하는데, 나는 저녁 12시가 되기전 11시 55분에 매일 Lambda에 이벤트를 줄 예정이기 때문에 다음과 같이 설정했다. Cron식이 궁금하다면 [AWS CloudWatch로 자동 카운트 시스템 개발하기 -1](https://liampoet.github.io/Lambda-CloudWatch/)을 보면된다. 

![_config.yml]({{ site.baseurl }}/images/규칙2 일정패턴.png)

GMT는 그리니치 평균시(Greenwich Mean Time)라고 하는데, 그냥 대한민국 서울의 시간이 필요한 나는 GMT에 9시간을 더한 현지 시간대를 선택했다.
<br>
<br>

***3단계 - 대상 선택***

대상 유형에는 각각 EventBridge 이벤트 버스, [EventBridge API대상](https://docs.aws.amazon.com/ko_kr/eventbridge/latest/userguide/eb-api-destinations.html), AWS 서비스가 있다. 하나의 event rule에 여러개의 대상이 등록이 가능하며, 나는 Lambda함수에 이벤트를 줄거라서 AWS서비스의 Lambda 항수를 선택했다. 

![_config.yml]({{ site.baseurl }}/images/규칙3-추가설정.png)

![_config.yml]({{ site.baseurl }}/images/규칙5 생성.png)

그 후 규칙을 생성하면 아래와 같이 생성한 규칙의 세부 정보와 상태를 바로 확인할 수 있다.

![_config.yml]({{ site.baseurl }}/images/규칙마지막.png)
<br>
<br>

**[참고자료]**
- [https://www.nextree.co.kr/p4327/](https://www.nextree.co.kr/p4327/)
