---
layout: post
title: AWS Lambda에 Bootpay연동하기 - 결제검증
category: Blog
tags: [Git, AWS Lambda, Bootpay]
---
BootPay는 무료로 서비스되는 결제검증API이다.<br>
초기에 일반 PG사와 별도의 계약없이 개발을 먼저 할 수 있어서, 앱결제가 급하게 필요했던 나는 BootPay를 이용하기로 했다.(매우간단하다!)

[부트페이 개발문서](https://docs.bootpay.co.kr)
---------------------------------------


* 결제 검증을 해야하는 이유

결제 연동은 Client Side에서 동작되기 때문에 결제 금액 및 결제 상태에 대한 변조가 가능하다.<br>
그러므로 반드시 처음 요청했던 금액과 결제가 올바르게 이루어졌는지에 대해 서버사이드에서 서버로 영수증 키를 보내 확인하는 과정을 거쳐야 한다고 한다.

* 그렇다면 결제 검증을 무조건 해야하는가?

결제 검증을 하지 않더라도 PG사에서 제공하는 결제 로직을 모두 완료할 수 있다.<br>
방금 말했다시피, 실제 결제가 이루어지지만 Client Side에서 언제든 변조된 데이터를 구현하신 서버로 보내 부정적인 방법으로 구매 완료 데이터를 보낼 수 있기 때문에,<br>
결제검증은 필수로 하는것이 좋다고 부트페이에서는 설명한다.
<br>
<br>
### 1. Lambda layer생성
---------------------------------------

[부트페이 결제 및 취소 모듈(python)](https://github.com/bootpay/server_python)

부트페이 모듈을 다운 받은 뒤, Lambda layer를 생성해준다.

![_config.yml]({{ site.baseurl }}/images/Lambda_layer.png)
<br>
<br>
### 2. Add a layer
---------------------------------------

생성한 부트페이 layer를 Lambda함수에 추가해준다.

![_config.yml]({{ site.baseurl }}/images/Add_layer.png)

```python
from lib.BootpayApi import BootpayApi
```
<br>
<br>
### 3. 검증 코드 작성하기
---------------------------------------

<br>
```python
from lib.BootpayApi import BootpayApi

bootpay = BootpayApi(
    '[[ application_id ]]',
    '[[ private_key ]]'
)

result = bootpay.get_access_token()
if result['status'] is 200:
    verify_result = bootpay.verify('[[ receipt_id ]]')
    if verify_result['status'] is 200:
        # 원래 주문했던 금액이 일치하는가?
        # 그리고 결제 상태가 완료 상태인가?
        if verify_result['data']['status'] is 1 and verify_result['data']['price'] is price:
            # TODO: 이곳이 상품 지급 혹은 결제 완료 처리를 하는 로직으로 사용하면 됩니다.
```

이것만 작성하면 매우 간단하게 결제 검증을 끝낼 수 있다.<br>
필자는 웹결제가 없었기 떄문에 앱에서 결제 후 서버사이드에서 결제 검증만 진행했다.<br>
application_id와 private_key는 부트페이 관리자에서 확인할 수 있다.<br>
또한 위 코드와 같이 결제 금액으로 비교를 해주면 되는데, 가격말고도 원하는 다른 데이터를 추가로 비교할 수 도있다<br>
<br>
* 검증결과

```json
{
  "status": 200,
  "code": 0,
  "message": "",
  "data": {
    "receipt_id": "5afd6be8e13f33616f2876ac",
    "order_id": "1526557671321",
    "name": "테스트음식",
    "price": 3000,
    "unit": "krw",
    "pg": "kcp",
    "method": "card",
    "pg_name": "KCP",
    "method_name": "카드결제",
    "payment_data": {
      "card_name": "BC카드",
      ...,
      "s": 1,
      "g": 2
    },
    "requested_at": "2018-05-17 20:47:52",
    "purchased_at": "2018-05-17 20:48:53",
    "status": 1
  }
}
```py
<br>
결제가 완료되면 "s"값이 1이나온다.<br>
"status"의 경우 현재 결제의 상태를 보여주는데, 다음과 같이 나타낼 수 있다.
<br>
```
0 - 결제 대기 상태. 승인이 나기 전의 상태.
1 - 결제 완료된 상태.
2 - 결제승인 전 상태. transactionConfirm() 함수를 호출하셔서 결제를 승인해야함.
3 - 결제승인 중 상태. PG사에서 transaction 처리 중인 상태.
20 - 결제가 취소된 상태.
-20 - 결제취소가 실패한 상태.
-30 - 결제취소가 진행중인 상태.
-1 - 오류로 인해 결제가 실패한 상태.
-2 - 결제승인이 실패함.
```
<br>
서류작성 및 PG사 심사(앱 결제테스트 등)는 거의 3주 가까이 걸렸는데,<br>
서버사이드의 작업은 3시간도 채 걸리지 않고 작업을 끝낼 수 있다.<br>
또한 부트페이 개발자와 원할한 소통도 가능하고 Bootpay 버그에 대한 수정 요청도 빠르게 처리해 주기 때문에,(필자는 앱단에서 요청사항이 있었는데, 4일내로 업데이트 해주셨다.)
나처럼 빠른시일내 앱결제 시스템이 필요한경우 Bootpay를 한번 고려해보기 바란다! :-)
