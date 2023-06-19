# 메일 변환 결과 알림

## 메일 변환 결과 알림

{% swagger method="post" path="/v1/shieldrm/{requestID}" baseUrl="resultUrl" summary="SHIELDRM 을 통해 메일 변환후 결과를 알려줄 API (SHIELDRM for Mail)" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="header" type="json" %}
헤더 변환 필요시 적용할 헤더 정보
{% endswagger-parameter %}

{% swagger-parameter in="body" name="msg" type="json" %}
본문 변환 필요시 적용할 본문 정보
{% endswagger-parameter %}

{% swagger-parameter in="body" name="files" type="array json" %}
변환된 파일 목록
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="정상 처리" %}

{% endswagger-response %}

{% swagger-response status="404: Not Found" description="requestID 가 없음" %}

{% endswagger-response %}

{% swagger-response status="500: Internal Server Error" description="내부 처리 오류" %}

{% endswagger-response %}
{% endswagger %}

#### 메일의 헤더 JSON 형식

<table><thead><tr><th>Name</th><th width="143">Type</th><th width="305">설명</th><th>필수</th><th data-hidden></th></tr></thead><tbody><tr><td>subject</td><td>string</td><td>메일 제목</td><td>선택</td><td></td></tr><tr><td>from</td><td>string</td><td>보낸 사람</td><td>선택</td><td></td></tr><tr><td>to</td><td>array string</td><td>받는 사람 목록</td><td>선택</td><td></td></tr><tr><td>cc</td><td>array string</td><td>참조 목록</td><td>선택</td><td></td></tr></tbody></table>

#### 메일의 본문 JSON 형식

<table><thead><tr><th width="162">Name</th><th width="141">Type</th><th width="296">설명</th><th>필수</th><th data-hidden></th></tr></thead><tbody><tr><td>msg</td><td>string</td><td>메일 본문</td><td>선택</td><td></td></tr><tr><td>format</td><td>string</td><td>msg 의  형식  (html 또는 text)</td><td>선택</td><td></td></tr></tbody></table>

#### 메일에 첨부된 파일 목록의JSON 형식

<table><thead><tr><th width="162">Name</th><th width="141">Type</th><th width="296">설명</th><th>필수</th><th data-hidden></th></tr></thead><tbody><tr><td>name</td><td>string</td><td>메일 제목</td><td>필수</td><td></td></tr><tr><td>path</td><td>string</td><td>보낸 사람</td><td>필수</td><td></td></tr><tr><td>code</td><td>int</td><td>처리결과 코드</td><td>필수</td><td></td></tr><tr><td>desc</td><td>string</td><td>처리결과 설명</td><td>필수</td><td></td></tr></tbody></table>

