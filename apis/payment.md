# 결제관련 API



## 목차

[1.결제수단리스트-API](#결제수단리스트-API)</br>

# 결제수단리스트 API

## Request

##### URL(User Access Token 사용)

```http
POST /v1/payment/methods HTTP/1.1
Host: dapi.o2obusan.com
Content-type: application/x-www-form-urlencoded;charset=utf-8
```



##### Header

| Name           | Type     | Description              | Required |
| :------------- | :------- | :----------------------- | :------- |
| AppId          | `String` | 인증서버에서 발급받은 Id | O        |
| Authentication | `String` | User Access Token        | O        |



## Response

| Name          | Type      | Length | Description |
| :------------ | :-------- | ------ | :---------- |
| resultCode    | `String`  | 4      | 결과코드    |
| resultMessage | `String`  | 100    | 결과메시지  |
| methodId      | `Integer` | 1      | 결제수단ID  |
| methodName    | `String`  | 100    | 결제수단명  |



## Sample

### Request

```shell
curl -v -X POST http://dapi.o2obusan.com/v1/payment/methods
```



### Response

```java
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{
    "resultCode": "0000",
    "resultMessage": "OK",
    "data": [
        {
            "methodId": 1,
            "methodName": "카드결제"
        },
        {
            "methodId": 2,
            "methodName": "KB간편결제"
        },
        {
            "methodId": 3,
            "methodName": "동백전"
        }
    ]
}
```



### Sample Code

#### Java

```java
HttpHeaders headers = new HttpHeaders();
headers.add("Authorization", String.format("Bearer %s", token));

HttpEntity httpEntity = new HttpEntity(null, headers);

RestTemplate restTemplate = new RestTemplate();
ResponseEntity<PaymentMethodDto> result = restTemplate.exchange("http://dapi.o2obusan.com/v1/payment/methods", HttpMethod.GET, httpEntity, PaymentMethodDto.class);

if(result.getBody() == null){
    return;
}

PaymentMethodDto paymentMethodDto = result.getBody();
```


