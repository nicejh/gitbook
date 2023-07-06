---
description: 메일을 조건부 정책에 따라 변환 요청 하는 API 이며 다중 수신자는 수신자 개인별로 API 를 호출한다
---

# 메일 변환 요청

## 메일 변환 요청

{% swagger baseUrl="https://api.myapi.com/v1" method="put" path="/mail/" summary="메일 변환 요청" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="id" required="true" type="string" %}
요청 ID
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ip" type="string" required="true" %}
SHIELDRM for Mail 로 연결한 IP
{% endswagger-parameter %}

{% swagger-parameter in="body" name="tenantID" type="string" required="true" %}
고객사 ID
{% endswagger-parameter %}

{% swagger-parameter in="body" name="sender" required="true" type="string" %}
메일 송신자 Email
{% endswagger-parameter %}

{% swagger-parameter in="body" name="recver" required="true" type="string" %}
메일 수신자 Email
{% endswagger-parameter %}

{% swagger-parameter in="body" name="header" required="false" type="json" %}
메일의 헤더
{% endswagger-parameter %}

{% swagger-parameter in="body" name="msg" type="json" required="false" %}
메일의 본문
{% endswagger-parameter %}

{% swagger-parameter in="body" name="files" type="array json" required="true" %}
메일에 첨부된 파일 목록
{% endswagger-parameter %}

{% swagger-parameter in="body" name="urlResponse" type="string" required="true" %}
메일 변환 처리 완료후 응답할 URL
{% endswagger-parameter %}

{% swagger-parameter in="body" name="time" type="timestamp" required="true" %}
Unix Timestamp (초단위)
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

<table><thead><tr><th width="155">Name</th><th width="138">Type</th><th width="327">설명</th><th>필수</th><th data-hidden></th></tr></thead><tbody><tr><td>subject</td><td>string</td><td>메일 제목</td><td>선택</td><td></td></tr><tr><td>from</td><td>string</td><td>보낸 사람</td><td>선택</td><td></td></tr><tr><td>to</td><td>array string</td><td>받는 사람 목록</td><td>선택</td><td></td></tr><tr><td>cc</td><td>array string</td><td>참조 목록</td><td>선택</td><td></td></tr></tbody></table>

#### 메일의 본문 JSON 형식

<table><thead><tr><th width="162">Name</th><th width="141">Type</th><th width="296">설명</th><th>필수</th><th data-hidden></th></tr></thead><tbody><tr><td>msg</td><td>string</td><td>메일 본문</td><td>선택</td><td></td></tr><tr><td>format</td><td>string</td><td>msg 의 형식 (html 또는 text)</td><td>선택</td><td></td></tr></tbody></table>

#### 메일에 첨부된 파일 목록의JSON 형식

<table><thead><tr><th width="162">Name</th><th width="141">Type</th><th width="296">설명</th><th>필수</th><th data-hidden></th></tr></thead><tbody><tr><td>name</td><td>string</td><td>파일명</td><td>필수</td><td></td></tr><tr><td>path</td><td>string</td><td>파일 경로</td><td>필수</td><td></td></tr></tbody></table>
