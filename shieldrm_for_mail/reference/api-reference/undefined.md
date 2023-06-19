---
description: 메일을 조건부 정책에 따라 변환 요청 받는 API
---

# 메일 변환 요청

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

<table><thead><tr><th width="162">Name</th><th width="141">Type</th><th width="296">설명</th><th>필수</th><th data-hidden></th></tr></thead><tbody><tr><td>subject</td><td>string</td><td>메일 제목</td><td>선택</td><td></td></tr><tr><td>from</td><td>string</td><td>보낸 사람</td><td>선택</td><td></td></tr><tr><td>to</td><td>array string</td><td>받는 사람 목록</td><td>선택</td><td></td></tr><tr><td>cc</td><td>array string</td><td>참조 목록</td><td>선택</td><td></td></tr></tbody></table>

#### 메일의 본문 JSON 형식

<table><thead><tr><th width="162">Name</th><th width="141">Type</th><th width="296">설명</th><th>필수</th><th data-hidden></th></tr></thead><tbody><tr><td>msg</td><td>string</td><td>메일 본문</td><td>선택</td><td></td></tr><tr><td>format</td><td>string</td><td>msg 의  형식  (html 또는 text)</td><td>선택</td><td></td></tr></tbody></table>

#### 메일에 첨부된 파일 목록의JSON 형식

<table><thead><tr><th width="162">Name</th><th width="141">Type</th><th width="296">설명</th><th>필수</th><th data-hidden></th></tr></thead><tbody><tr><td>name</td><td>string</td><td>메일 제목</td><td>필수</td><td></td></tr><tr><td>path</td><td>string</td><td>보낸 사람</td><td>필수</td><td></td></tr></tbody></table>



## 메일 변환  진행상황  조회

{% swagger baseUrl="https://api.myapi.com/v1" method="get" path="/mail/{id}" summary="메일 변환 진행 상황 조회" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-response status="200: OK" description="완료됨" %}

{% endswagger-response %}

{% swagger-response status="204: No Content" description="처리중" %}

{% endswagger-response %}

{% swagger-response status="404: Not Found" description="해당 ID 가 없음" %}
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

{% swagger-response status="500: Internal Server Error" description="내부 처리 오류" %}

{% endswagger-response %}
{% endswagger %}

