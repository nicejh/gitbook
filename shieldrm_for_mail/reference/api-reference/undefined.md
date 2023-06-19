---
description: 메일을 조건부 정책에 따라 변환 요청 받는 API
---

# 메일 변환

## 메일 변환 요청

{% swagger baseUrl="https://api.myapi.com/v1" method="put" path="/mail/" summary="메일 변환 요청" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="id" required="true" type="string" %}
요청 ID
{% endswagger-parameter %}

{% swagger-parameter in="body" name="sender" required="true" type="string" %}
메일 송신자 Email
{% endswagger-parameter %}

{% swagger-parameter in="body" name="recvers" required="true" type="array string" %}
메일 수신자 Email 목록
{% endswagger-parameter %}

{% swagger-parameter in="body" name="header" required="true" type="json" %}
메일의 헤더
{% endswagger-parameter %}

{% swagger-parameter in="body" name="msg" type="json" %}
메일의 본문
{% endswagger-parameter %}

{% swagger-parameter in="body" name="files" type="array json" required="true" %}
메일에 첨부된 파일 목록
{% endswagger-parameter %}

{% swagger-response status="200" description="요청이 접수됨" %}
```javascript
{
    "name"="Wilson",
    "owner": {
        "id": "sha7891bikojbkreuy",
        "name": "Samuel Passet",
    "species": "Dog",}
    "breed": "Golden Retriever",
}
```
{% endswagger-response %}

{% swagger-response status="400" description="요청 파라미터 오류" %}

{% endswagger-response %}

{% swagger-response status="500: Internal Server Error" description="내부 처리 오류" %}

{% endswagger-response %}
{% endswagger %}

#### 메일의 헤더 JSON 형식



## 메일 변환  진행상황  조회

{% swagger baseUrl="https://api.myapi.com/v1" method="get" path="/mail/{id}" summary="메일 변환 진행상황 조회" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="id" required="true" type="string" %}
요청 ID
{% endswagger-parameter %}

{% swagger-parameter in="body" name="sender" required="true" type="string" %}
메일 송신자 Email
{% endswagger-parameter %}

{% swagger-parameter in="body" name="recvers" required="true" type="array string" %}
메일 수신자 Email 목록
{% endswagger-parameter %}

{% swagger-parameter in="body" name="header" required="true" type="json" %}
메일의 헤더
{% endswagger-parameter %}

{% swagger-parameter in="body" name="msg" type="json" %}
메일의 본문
{% endswagger-parameter %}

{% swagger-parameter in="body" name="files" type="array json" required="true" %}
메일에 첨부된 파일 목록
{% endswagger-parameter %}

{% swagger-response status="200" description="요청이 접수됨" %}
```javascript
{
    "name"="Wilson",
    "owner": {
        "id": "sha7891bikojbkreuy",
        "name": "Samuel Passet",
    "species": "Dog",}
    "breed": "Golden Retriever",
}
```
{% endswagger-response %}

{% swagger-response status="400" description="요청 파라미터 오류" %}

{% endswagger-response %}

{% swagger-response status="500: Internal Server Error" description="내부 처리 오류" %}

{% endswagger-response %}
{% endswagger %}

{% hint style="info" %}
**Good to know:** This API method was created using the API Method block, it's how you can build out an API method documentation from scratch. Have a play with the block and you'll see you can do some nifty things like add and reorder parameters, document responses, and give your methods detailed descriptions.
{% endhint %}

