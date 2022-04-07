---
layout: post
title: AWS CloudWatch로 자동 카운트 시스템 개발하기 -2
category: Blog
tags: [Git, AWS Lambda, AWS CloudWatch, Python]
etc: Amazon EventBridge cron 설정하기
---
## 💡 Intro
- Lambda함수를 이용해 특정시간에 자동으로 카운트를 해야하는 방법이 필요해졌다!
- AWS Lambda는 실시간으로 코드를 실행할 수는 있으나, 필요시에만 함수를 실행한다. Lambda에 있는 함수를 자동으로 동작시킬 수 있는 기술을 찾던 중, 나는 Amazon CloudWatch에 있는 EventBridge를 알게되었다.
<br>
<br>
<br>

## 🌩 Amazon EventBridge 규칙 생성하기
---------------------------------------
### 1. Amazon EventBridge 규칙 생성하기

AWS 홈페이지에서 Amazon EventBridge를 들어간다.

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
- [https://velog.io/@techy-yunong/AWS-EventBridge-concept](https://velog.io/@techy-yunong/AWS-EventBridge-concept)
- [Amazon EventBridge 이벤트 버스 생성](https://docs.aws.amazon.com/ko_kr/eventbridge/latest/userguide/eb-create-event-bus.html)
