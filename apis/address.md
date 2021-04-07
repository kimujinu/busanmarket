# 주소 API





## 목차

[1.사용자 배송주소 조회 API](#사용자 배송주소 조회 API)</br>
[2.주문 등록 API](#주문-등록-API)</br>

# 사용자 배송주소 조회 API

## Request

##### URL(User Access Token 사용)

```http
GET /v1/address/group/{group} HTTP/1.1
Host: dapi.o2obusan.com
Content-type: application/x-www-form-urlencoded;charset=utf-8
```



##### Header

| Name           | Type     | Description                                            | Required |
| :------------- | :------- | :----------------------------------------------------- | :------- |
| AppId          | `String` | 인증서버에서 발급받은 Id                               | O        |
| Authentication | `String` | 토큰 (User Access Token 또는 Application Access Token) | O        |
| UserUuid       | `String` | Application Access Token을 이용할 경우 필수            |          |



##### Pathvariable

| Name  | Type      | Length | Description                                       | Required |
| :---- | :-------- | ------ | :------------------------------------------------ | :------- |
| group | `Integer` | 1      | 주소그룹 (음식배달 : 1, 전통시장 : 2, 쇼핑몰 : 3) | O        |



## Response

| Name          | Type     | Length | Description                                       |
| :------------ | :------- | ------ | :------------------------------------------------ |
| resultCode    | `String` | 4      | 결과코드                                          |
| resultMessage | `String` | 100    | 결과메시지                                        |
| data          | `Object` |        | 요청한 Data                                       |
| addressGroup  | `String` | 1      | 주소그룹 (음식배달 : 1, 전통시장 : 2, 쇼핑몰 : 3) |
| x             | `String` | 100    | x좌표                                             |
| y             | `String` | 10     | y좌표                                             |
| baseAddress   | `String` | 16     | 지번 주소                                         |
| streetAddress | `String` | 100    | 도로명 주소                                       |
| buildingName  | `String` | 100    | 건물명                                            |
| detailAddress | `String` | 255    | 상세 주소                                         |
|               |          |        |                                                   |



## Sample

### Request

```shell
curl -v -X GET http://dapi.o2obusan.com/v1/address/group/{group}
```



### Response

```java
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{
    "resultCode": "0000",
    "resultMessage": "OK",
    "data": {
        "addressGroup": "1",
        "x": "37.4892876809567",
        "y": "127.068344314779",
        "baseAddress": "서울특별시 강남구 개포동 186-16 현대빌딩",
        "streetAddress": "개포로 510",
        "buildingName": "현대빌딩",
        "detailAddress": "1층 편의점"
    }
}
```



### Sample Code

#### Java

```java
HttpHeaders headers = new HttpHeaders();
headers.add("Authorization", String.format("Bearer %s", token));

HttpEntity httpEntity = new HttpEntity(null, headers);

RestTemplate restTemplate = new RestTemplate();
ResponseEntity<AddressDto> result = restTemplate.exchange("http://dapi.o2obusan.com/v1/address/group/1", HttpMethod.GET, httpEntity, ProfileBaseDto.class);

if(result.getBody() == null){
    return;
}

AddressDto addressDto = result.getBody();
```


