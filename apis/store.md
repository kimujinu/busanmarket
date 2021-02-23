[TOC]



## 매장 정보 생성 API

#### Request

###### URL

``` 
POST /store
Content-Type: application/json
```

###### Header


| Name          | Type                | Description                            | Required |
| :------------ | ------------------- | -------------------------------------- | -------- |
| Authorization | <code>String</code> | 인증토큰 <code>Bearer + {token}</code> | O        |
| AppId         | `String`            | 인증서버에서 발급받은 Id               | O        |

###### Parameter

| Name               | Type                | Length | Description      | Required |
| ------------------ | ------------------- | ------ | ---------------- | -------- |
| name               | <code>String</code> | 100    | 매장 이름        | O        |
| tel                | <code>String</code> | 13     | 매장 전화번호    | O        |
| open               | <code>String</code> | 10     | 매장 오픈 시간   | O        |
| close              | <code>String</code> | 10     | 매장 마감 시간   | O        |
| addr               | <code>String</code> | 100    | 매장 주소        | O        |
| companyRegisterNum | <code>String</code> | 12     | 사업자 등록 번호 | O        |



#### Response 

| Name               | Type     | Length | Description      |
| ------------------ | -------- | ------ | ---------------- |
| resultCode         | `String` | 4      | 결과코드         |
| resultMessage      | `String` | 100    | 결과메시지       |
| id                 | `String` | 40     | 매장 ID          |
| name               | `String` | 100    | 매장 명          |
| tel                | `String` | 13     | 매장 전화번호    |
| open               | `String` | 10     | 매장 오픈 시간   |
| close              | `String` | 10     | 매장 마감 시간   |
| addr               | `String` | 100    | 매장 주소        |
| companyRegisterNum | `String` | 12     | 사업자 등록 번호 |



#### Sample

###### Request

```
curl -L -X POST 'localhost:8086/store?name={name}&tel={tel}&open={open}&close={close}&addr={addr}&companyRegisterNum={companyRegisterNum}' -H 'Authorization: {token}' -H 'Content-Type: application/json'
```

###### Response

```
HTTP/1.1 200 OK
{
    "resultCode": "0000",
    "resultMessage": "OK",
    "id": "e1a4df4a937c463b966aa7378bde8679",
    "name": "moringTest",
    "tel": "2020",
    "open": "00:00",
    "close": "23:59",
    "addr": "moringTest",
    "companyRegisterNum": "123-123-1234"
}
```

###### Sample Code


```java
String clientId = "dev-shop";
MultiValueMap<String, String> body = new LinkedMultiValueMap<>();
body.add("name", "김가네");
body.add("tel", "010-1234-5678");
body.add("open", "00:00");
body.add("close", "23:59");
body.add("addr", "서울시 종로구 한탄로 102길 32");
body.add("compayRegisterNum", "000-00-00000");

HttpHeaders headers = new HttpHeaders();
headers.add("Authorization", String.format("Bearer %s", token));
headers.add("Content-Type", "application/json");
headers.add("AppId", clientId)

HttpEntity httpEntity = new HttpEntity(body, headers);
RestTemplate restTemplate = new RestTemplate();

ResponseEntity<StoreDto> result = restTemplate.postForEntity("/store", httpEntity, StoreDto.class);
```



## 전체 매장 정보 조회 API

#### Request

###### URL

``` 
GET /store
Content-Type: application/json
```

###### Header


| Name          | Type                | Description                            | Required |
| :------------ | ------------------- | -------------------------------------- | -------- |
| Authorization | <code>String</code> | 인증토큰 <code>Bearer + {token}</code> | O        |
| AppId         | `String`            | 인증서버에서 발급받은 Id               | O        |
| Content-Type  | <code>String</code> | 반환되는 데이터의 형식                 |          |

###### Parameter

| Name | Type | Length | Description | Required |
| ---- | ---- | ------ | ----------- | -------- |
|      |      |        |             |          |



#### Response

| Name               | Type     | Length | Description      |
| ------------------ | -------- | ------ | ---------------- |
| resultCode         | `String` | 4      | 결과코드         |
| resultMessage      | `String` | 100    | 결과메시지       |
| id                 | `String` | 40     | 매장ID           |
| name               | `String` | 100    | 상점 이름        |
| tel                | `String` | 13     | 매장 전화번호    |
| open               | `String` | 10     | 매장 오픈 시간   |
| close              | `String` | 10     | 매장 마감 시간   |
| addr               | `String` | 100    | 매장 주소        |
| companyRegisterNum | `String` | 12     | 사업자 등록 번호 |



#### Sample 

###### Request 

```
curl -L -X GET 'localhost:8086/store' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json'
```

###### Response 

```
HTTP/1.1 200 OK
[
    {
        "resultCode": "0000",
        "resultMessage": "OK",
        "id": "c7432908663848c9ac628a5649589d51",
        "name": "moringTest",
        "tel": "2020",
        "open": "00:00",
        "close": "23:59",
        "addr": "moringTest",
        "companyRegisterNum": "123-123-1234"
    },
    {
        "resultCode": "0000",
        "resultMessage": "OK",
        "id": "e1a4df4a937c463b966aa7378bde8679",
        "name": "moringTest",
        "tel": "2020",
        "open": "00:00",
        "close": "23:59",
        "addr": "moringTest",
        "companyRegisterNum": "123-123-1234"
    }
]
```

###### Sample Code 


```java
String clientId = "dev-shop";
MultiValueMap<String, String> body = new LinkedMultiValueMap<>();

HttpHeaders headers = new HttpHeaders();
headers.add("Authorization", String.format("Bearer %s", token));
headers.add("Content-Type", "application/json");
headers.add("AppId", clientId)

HttpEntity httpEntity = new HttpEntity(body, headers);
RestTemplate restTemplate = new RestTemplate();

ResponseEntity<StoreDto> result = restTemplate.getForEntity("/store", httpEntity, StoreDto.class);
```



## 매장 정보 조회 API

#### Request 

###### URL 

```
GET /store/{id}
Content-Type: application/json
```

###### Header 

| Name          | Type                | Description                            | Required |
| :------------ | ------------------- | -------------------------------------- | -------- |
| Authorization | <code>String</code> | 인증토큰 <code>Bearer + {token}</code> | O        |
| AppId         | `String`            | 인증서버에서 발급받은 Id               | O        |
| Content-Type  | <code>String</code> | 반환되는 데이터의 형식                 |          |

###### Parameter 

| Name | Type | Length | Description | Required |
| ---- | ---- | ------ | ----------- | -------- |
|      |      |        |             |          |



#### Response 

| Name               | Type     | Length | Description      |
| ------------------ | -------- | ------ | ---------------- |
| resultCode         | `String` | 4      | 결과코드         |
| resultMessage      | `String` | 100    | 결과메시지       |
| id                 | `String` | 40     | 매장ID           |
| name               | `String` | 100    | 매장이름         |
| tel                | `String` | 13     | 매장 전화번호    |
| open               | `String` | 10     | 매장 오픈 시간   |
| close              | `String` | 10     | 매장 마감 시간   |
| addr               | `String` | 100    | 매장 주소        |
| companyRegisterNum | `String` | 12     | 사업자 등록 번호 |



#### Sample 

###### Request

```
curl -L -X GET 'localhost:8086/store/{id}' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json'
```

###### Response 

```
{
    "resultCode": "0000",
    "resultMessage": "OK",
    "id": "a2cb6b79a59c4802b62f2d9bb3b90673",
    "name": "moringTest",
    "tel": "2020",
    "open": "00:00",
    "close": "23:59",
    "addr": "moringTest",
    "companyRegisterNum": "123-123-1234"
}
```

###### Sample Code 


```java
String clientId = "dev-shop";
MultiValueMap<String, String> body = new LinkedMultiValueMap<>();

HttpHeaders headers = new HttpHeaders();
headers.add("Authorization", String.format("Bearer %s", token));
headers.add("Content-Type", "application/json");
headers.add("AppId", clientId)

HttpEntity httpEntity = new HttpEntity(body, headers);
RestTemplate restTemplate = new RestTemplate();

ResponseEntity<StoreDto> result = restTemplate.getForEntity("/store/0d7371bdbaf04f559d62f81b13b2d9f1", httpEntity, StoreDto.class);
```



## 매장 정보 수정 API

#### Request 

###### URL 

```
PUT /store/{id}
Content-Type: application/json
```

###### Header 


| Name          | Type                | Description                            | Required |
| :------------ | ------------------- | -------------------------------------- | -------- |
| Authorization | <code>String</code> | 인증토큰 <code>Bearer + {token}</code> | O        |
| AppId         | `String`            | 인증서버에서 발급받은 Id               | O        |
| Content-Type  | <code>String</code> | 반환되는 데이터의 형식                 |          |

###### Parameter

| Name               | Type                | Length | Description      | Required |
| ------------------ | ------------------- | ------ | ---------------- | -------- |
| name               | <code>String</code> | 100    | 매장이름         | O        |
| tel                | <code>String</code> | 13     | 매장 전화번호    | O        |
| open               | <code>String</code> | 10     | 매장 오픈 시간   | O        |
| close              | <code>String</code> | 10     | 매장 마감 시간   | O        |
| addr               | <code>String</code> | 100    | 매장 주소        | O        |
| companyRegisterNum | <code>String</code> | 12     | 사업자 등록 번호 | O        |



#### Response 

| Name               | Type     | Length | Description      |
| ------------------ | -------- | ------ | ---------------- |
| resultCode         | `String` | 4      | 결과코드         |
| resultMessage      | `String` | 100    | 결과메시지       |
| id                 | `String` | 40     | 매장ID           |
| name               | `String` | 100    | 매장 이름        |
| tel                | `String` | 13     | 매장 전화번호    |
| open               | `String` | 10     | 매장 오픈 시간   |
| close              | `String` | 10     | 매장 마감 시간   |
| addr               | `String` | 100    | 매장 주소        |
| companyRegisterNum | `String` | 12     | 사업자 등록 번호 |

#### Sample

###### Request

```
curl -L -X PUT 'localhost:8086/store/{id}?name={name}&tel={tel}&open={open}&companyRegisterNum={companyRegisterNum}&close={close}&addr={addr}' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json'
```

###### Response 

```
HTTP/1.1 200 OK
{
    "resultCode": "0000",
    "resultMessage": "OK",
    "id": "ae6c908bf55943f2aecc409cd86f7b48",
    "name": "morningModify",
    "tel": "2020",
    "open": "00:00",
    "close": "23:59",
    "addr": "moringTest",
    "companyRegisterNum": "123-123-1234"
}
```

###### Sample Code 


```java
String clientId = "dev-shop";
MultiValueMap<String, String> body = new LinkedMultiValueMap<>();
body.add("name", "김가네");
body.add("tel", "010-1234-5678");
body.add("open", "00:00");
body.add("close", "23:59");
body.add("addr", "서울시 종로구 한탄로 102길 32");
body.add("compayRegisterNum", "000-00-00000");

HttpHeaders headers = new HttpHeaders();
headers.add("Authorization", String.format("Bearer %s", token));
headers.add("Content-Type", "application/json");
headers.add("AppId", clientId)

HttpEntity httpEntity = new HttpEntity(body, headers);
RestTemplate restTemplate = new RestTemplate();

ResponseEntity<StoreDto> result = restTemplate.putForEntity("/store/0d7371bdbaf04f559d62f81b13b2d9f1", httpEntity, StoreDto.class);
```



## 매장 정보 삭제 API

#### Request

###### URL

```
DELETE /store/{id}
Content-Type: application/json
```

###### Header

| Name          | Type                | Description                            | Required |
| :------------ | ------------------- | -------------------------------------- | -------- |
| Authorization | <code>String</code> | 인증토큰 <code>Bearer + {token}</code> | O        |
| AppId         | `String`            | 인증서버에서 발급받은 Id               | O        |
| Content-Type  | <code>String</code> | 반환되는 데이터의 형식                 |          |

###### Parameter

| Name | Type | Length | Description | Required |
| ---- | ---- | ------ | ----------- | -------- |
|      |      |        |             |          |



#### Response

| Name               | Type     | Length | Description      |
| ------------------ | -------- | ------ | ---------------- |
| resultCode         | `String` | 4      | 결과코드         |
| resultMessage      | `String` | 100    | 결과메시지       |
| id                 | `String` | 40     | 매장ID           |
| name               | `String` | 100    | 매장 이름        |
| tel                | `String` | 13     | 매장 전화번호    |
| open               | `String` | 10     | 매장 오픈 시간   |
| close              | `String` | 10     | 매장 마감 시간   |
| addr               | `String` | 100    | 매장 주소        |
| companyRegisterNum | `String` | 12     | 사업자 등록 번호 |

#### Sample

###### Request

``` 
curl -L -X DELETE 'localhost:8086/store/{id}' \
--header 'Authorization: Bearer {token}' \
--header 'Content-Type: application/json'
```

###### Response 

``` 
{
    "resultCode": "0000",
    "resultMessage": "OK",
    "id": "ae6c908bf55943f2aecc409cd86f7b48",
    "name": "morningModify",
    "tel": "2020",
    "open": "00:00",
    "close": "23:59",
    "addr": "moringTest",
    "companyRegisterNum": "123-123-1234"
}
```

###### Sample Code 


```java
String clientId = "dev-shop";
MultiValueMap<String, String> body = new LinkedMultiValueMap<>();

HttpHeaders headers = new HttpHeaders();
headers.add("Authorization", String.format("Bearer %s", token));
headers.add("Content-Type", "application/json");
headers.add("AppId", clientId)

HttpEntity httpEntity = new HttpEntity(body, headers);
RestTemplate restTemplate = new RestTemplate();

ResponseEntity<StoreDto> result = restTemplate.deleteForEntity("/store/0d7371bdbaf04f559d62f81b13b2d9f1", httpEntity, StoreDto.class);
```



------

## 반환코드정보

| http status | resultCode | resultMessage                         |
| ----------- | ---------- | :------------------------------------ |
| 200         | 0000       | 정상                                  |
| 400         | 3001       | 매장명이 올바르지 않습니다.           |
| 400         | 3002       | 매장 전화번호가 올바르지 않습니다.    |
| 400         | 3003       | 매장 오픈시간이 올바르지 않습니다.    |
| 400         | 3004       | 매장 마감시간이 올바르지 않습니다.    |
| 400         | 3005       | 매장 주소가 올바르지 않습니다.        |
| 400         | 3006       | 사업자 등록 번호가 올바르지 않습니다. |
| 400         | 3007       | 모든 매장정보가 비어있습니다.         |
| 400         | 3008       | 일치하는 매장이 없습니다.             |









