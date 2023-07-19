---
description: 메일을 조건부 정책에 따라 변환 요청 하는 API 이며 다중 수신자는 수신자 개인별로 API 를 호출한다
---

# 메일 변환 요청

## 메일 변환 요청

{% swagger baseUrl="https://api.myapi.com/v1" method="put" path="/mail/{id}" summary="메일 변환 요청" %}
{% swagger-description %}

{% endswagger-description %}

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

{% swagger-parameter in="body" name="header.subject" type="string" %}
메일 제목
{% endswagger-parameter %}

{% swagger-parameter in="body" name="header.from" type="json" %}
보낸사람 정보
{% endswagger-parameter %}

{% swagger-parameter in="body" name="header.from.name" type="string" %}
보낸사람 이름
{% endswagger-parameter %}

{% swagger-parameter in="body" name="header.from.addr" type="string" %}
보낸사람 메일 주소
{% endswagger-parameter %}

{% swagger-parameter in="body" name="header.to" type="array json" %}
받는 사람 목록
{% endswagger-parameter %}

{% swagger-parameter in="body" name="header.to[].name" type="string" %}
받는사람 이름
{% endswagger-parameter %}

{% swagger-parameter in="body" name="header.to[].addr" type="string" %}
받는사람 메일 주소
{% endswagger-parameter %}

{% swagger-parameter in="body" name="header.cc[]" type="array json" %}
참조 목록
{% endswagger-parameter %}

{% swagger-parameter in="body" name="header.cc[].name" type="string" %}
참조 이름
{% endswagger-parameter %}

{% swagger-parameter in="body" name="header.cc[].addr" %}
참조 메일 주소
{% endswagger-parameter %}

{% swagger-parameter in="body" name="msg" type="json" required="false" %}
메일의 본문
{% endswagger-parameter %}

{% swagger-parameter in="body" name="msg.text" type="string" %}
메일 본문
{% endswagger-parameter %}

{% swagger-parameter in="body" name="msg.format" type="string" %}
msg 의 형식 (html 또는 text)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="files" type="array json" required="true" %}
메일에 첨부된 파일 목록
{% endswagger-parameter %}

{% swagger-parameter in="body" name="files[].name" type="string" %}
파일명
{% endswagger-parameter %}

{% swagger-parameter in="body" name="files[].path" %}
파일 경로
{% endswagger-parameter %}

{% swagger-parameter in="body" name="urlResponse" type="string" required="true" %}
메일 변환 처리 완료후 응답할 URL
{% endswagger-parameter %}

{% swagger-parameter in="body" name="time" type="timestamp" required="true" %}
Unix Timestamp (초단위)
{% endswagger-parameter %}

{% swagger-parameter in="header" name="authorization" type="string" required="true" %}
bearer JWT
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
