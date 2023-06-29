---
description: 메일 변환 요청된 안건에 대한 처리 완료여부 조회
---

# 메일 변환 진행상황 조회

{% swagger method="get" path="/v1 /mail/{id}" baseUrl="https://api.myapi.com" summary="메일 변환 진행 상황 조회" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-response status="200: OK" description="완료됨" %}

{% endswagger-response %}

{% swagger-response status="204: No Content" description="처리중" %}

{% endswagger-response %}

{% swagger-response status="404: Not Found" description="해당 ID 가 없음" %}

{% endswagger-response %}

{% swagger-response status="500: Internal Server Error" description="내부 처리 오류" %}

{% endswagger-response %}
{% endswagger %}
