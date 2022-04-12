---
layout: post
title: AWS CloudWatch로 자동 카운트 시스템 개발하기 -1
category: Blog
tags: [Git, AWS Lambda, AWS CloudWatch, Python]
etc: AWS Lambda는 실시간으로 코드를 실행할 수는 있으나, 필요시에만 함수를 실행한다. Lambda에 있는 함수를 자동으로 동작시킬 수 있는 기술을 찾던 중, 나는 Amazon CloudWatch에 있는 EventBridge를 알게되었다.
---
## 💡 Intro
- Lambda함수를 이용해 특정시간에 자동으로 카운트를 해야하는 방법이 필요해졌다!
- AWS Lambda는 실시간으로 코드를 실행할 수는 있으나, 필요시에만 함수를 실행한다. Lambda에 있는 함수를 자동으로 동작시킬 수 있는 기술을 찾던 중, 나는 Amazon CloudWatch에 있는 EventBridge를 알게되었다.
<br>
<br>
<br>

## 🔎 들어가기전, 알고가기! -1
---------------------------------------
**Lambda**
- [Lambda](https://docs.aws.amazon.com/ko_kr/lambda/latest/dg/welcome.html)는 서버를 프로비저닝하거나 관리하지 않고도 코드를 실행할 수 있게 해주는 컴퓨팅 서비스다. Lambda는 고가용성 컴퓨팅 인프라에서 코드를 실행하고 서버와 운영 체제 유지 관리, 용량 프로비저닝 및 자동 조정, 코드 및 보안 패치 배포, 코드 모니터링 및 로깅 등 모든 컴퓨팅 리소스 관리를 수행할 수 있다. 또한 Lambda는 필요 시에만 함수를 실행하며, 일일 몇 개의 요청에서 초당 수천 개의 요청까지 자동으로 확장이 가능하다. 사용한 컴퓨팅 시간만큼만 비용을 지불하고, 코드가 실행되지 않을 때는 요금이 부과되지 않는다.
<br>
<br>
**CloudWatch**
- [CloudWatch](https://docs.aws.amazon.com/ko_kr/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)는 Amazon Web Services(AWS) 리소스 및 AWS에서 실행되는 애플리케이션을 실시간으로 모니터링한다. CloudWatch를 사용하여 리소스 및 애플리케이션에 대해 측정할 수 있는 변수인 지표를 수집하고 추적할 수 있으며([Amazon CloudWatch Logs](https://docs.aws.amazon.com/ko_kr/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html)), [Amazon EventBridge](https://docs.aws.amazon.com/ko_kr/eventbridge/latest/userguide/eb-what-is.html)를 이용해 특정한 이벤트를 처리할 수 도 있다. 
- [EventBridge](https://docs.aws.amazon.com/ko_kr/eventbridge/latest/userguide/eb-what-is.html)는 과거에는  Amazon CloudWatch Events라고 불렸으며, Zendesk 또는 Shopify와 같은 이벤트 소스의 실시간 데이터 스트림을 AWS Lambda 및 기타 SaaS 애플리케이션과 같은 대상으로 전송하는 이벤트 기반 애플리케이션을 대규모로 손쉽게 구축할 수 있는 서버리스 이벤트 버스이다.
<br>
<br>
<br>

## 🔎 들어가기전, 알고가기! -2
---------------------------------------
**크론(Cron) 표현식이란?🤔**
- EventBridge는 이벤트를 세팅할때 기본적으로 크론 정규표현식을 사용한다. 그럼 크론식이 무엇일까?(나도 이번에 크론식을 처음 경험해 보았는데;-;, 한번씩 쓸 경우가 종종 생길 것 같아서 정리해두려고 한다.)
- 유닉스 계열의 운영체제에서 시간 기반으로 job scheduling을 하는 프로세스를 [크론 표현식(Cron Expressions)](https://docs.oracle.com/cd/E12058_01/doc/doc.1014/e12030/cron_expressions.htm)라고 하며 자바 스프링의 Quartz나 Spring Scheduler에서도 사용하기도 한다. 또한, 크론 표현식은 필드와 특수문자를 조합하여 스케쥴링을 조절할 수 있다.
- 리눅스/유닉스 크론 표현식에서는 5개의 필드를 사용하고, Quartz 기반의 크론 표현식에서는 7개의 필드를 사용하는 String 문자열이다. 공백으로 구분하고(" ") 각 필드는 앞에서 부터 초, 분, 시, 일, 월, 요일, 년으로 구분된다.
<br>
<br>

**크론 표현식 표현**<br>
![_config.yml]({{ site.baseurl }}/images/2022-04-04 크론표현식-표.png)
<br>
<br>

**크론 표현식 특수문자**
- * : 모든 값
- **?** : 특정한 값이 없음 
- **-** : 범위를 뜻 함 (예) 월요일에서 수요일까지는 MON-WED로 표현
- **,** : 특별한 값일 때만 동작 (예) 월,수,금 MON,WED,FRI 
- **/** : 시작시간 / 단위  (예) 0분부터 매 5분 0/5
- **L** : 일에서 사용하면 마지막 일, 요일에서는 마지막 요일(토요일)
- **W** : 가장 가까운 평일 (예) 15W는 15일에서 가장 가까운 평일 (월 ~ 금)을 찾음
- **#** : 몇째주의 무슨 요일을 표현 (예) 3#2 : 2번째주 수요일
<br>
<br>

**크론 표현식 예제**
![_config.yml]({{ site.baseurl }}/images/2022-04-04 크론표현식-예제.png)
- 크론 표현식에 대해 공부를 하면서 느낀점은 어느정도만 알면 전혀 어렵지 않다는 점이다. 표를 보고 대충 이해하여 한번만 만들어봐도 다음 부터는 쉽게 만들 수 있다.  
- [CronMaker](http://www.cronmaker.com/?1)라는 원하는 크론 표현식을 생성해 주는 사이트 존재한다. 대충 크론 표현식을 만든 후, 내가 만든 크론 표현식을 입력하고 Calcurate  next dates 버튼을 클릭하면 내가 만든 크론 표현식으로 언제 실행될것인지 미리 볼수 있는 기능 또한 존재한다.
<br>
<br>
<br>

## 📚 Lambda 코드 작성
---------------------------------------
### 📗 1. Lambda 함수생성
![_config.yml]({{ site.baseurl }}/images/2022-04-04 람다함수생성.png)
본인이 사용하는 언어를 선택한 후, Lambda 함수를 생성해준다. 그리고 VPC및 함수에 필요한 [Lambda Layer](https://docs.aws.amazon.com/ko_kr/lambda/latest/dg/configuration-layers.html)를 설정해준다.
<br>
<br>
### 📕 2. Lambda 코드작성
자동으로 카운트 해주어야할 데이터를 조건에 따라서 가짓 수를 나누어주었다. 나는 필요에 따라 S3와 RDS를 연결해주었다.  
<script src="https://gist.github.com/liampoet/fccac1660dbeb09082d2e82bf72fd28a.js"></script>



Amazon EventBridge 규칙 생성은 다음 포스팅에 이어서 작성을 해보도록 하겠다.😀

