# 사용자 정보 반환 API

현재 로그인한 사용자의 정보를 불러옵니다. 사용자 정보 요청 REST API는 사용자  토큰을 사용하는 방법, 앱 Client/Secret을 사용하는 방법 두 가지로 제공됩니다. 앱 Client/Secret은 보안에 유의하여 사용해야 하므로 서버에서 호출할 때만 사용합니다.

[1.User Access Token 이용](#User-Access-Token-이용)</br>
[2.Application Access Token 이용](#Application-Access-Token-이용)</br>


# User Access Token 이용

## Reequest

#### URL

```http
GET /v1/profile/me HTTP/1.1
Host: bastion.o2obusan.com
Authorization: Bearer {ACCESS_TOKEN}
Content-type: application/x-www-form-urlencoded;charset=utf-8
```

#### Header

| Name          | Description                                                  | Required |
| ------------- | ------------------------------------------------------------ | -------- |
| Authorization | 사용자 인증 수단, 액세스 토큰 값<br />Authorization: Bearer {ACCESS_TOKEN} | O        |



## Response

| Name      | Type     | Length | Description                                 |
| :-------- | :------- | ------ | :------------------------------------------ |
| uuid      | `String` | 40     | 회원 식별 id                                |
| id        | `String` | 0      | 회원 로그인 id (제공되지 않음.) 고정값 null |
| username  | `String` | 10     | 사용자 이름                                 |
| email     | `String` | 100    | 이메일주소                                  |
| çellphone | `String` | 13     | 휴대폰번호(010-1234-1234)                   |



## Sample

#### Request

``` shell
curl -v -X GET http://bastion.o2obusan.com/v1/profile/me \
  -H "Authorization: Bearer {ACCESS_TOKEN}"
```

#### Response

```json
HTTP/1.1 200 OK
{
    "uuid": "53w",
    "id": null,
    "username": "관리자",
    "email": "hong",
    "cellphone": "010-1234-0000"
}
```



## Sample source

### Java

```java
HttpHeaders headers = new HttpHeaders();
headers.add("Authorization", String.format("Bearer %s", token));

HttpEntity httpEntity = new HttpEntity(null, headers);

RestTemplate restTemplate = new RestTemplate();
ResponseEntity<ProfileBaseDto> result = restTemplate.exchange("http://bastion.o2obusan.com/v1/profile/me", HttpMethod.GET, httpEntity, ProfileBaseDto.class);

if(result.getBody() == null){
    return;
}
//사용자 정보 객체 조회
ProfileBaseDto resultProfileMe = result.getBody();
```



# Application Access Token 이용

client_id, secret을 통하여 발급받은 Application Access Token을 이용하여 사용자 정보를 조회하는 API 입니다.

## Reequest

#### URL

```http
GET /v1/profile/{USER_UUID} HTTP/1.1
Host: bastion.o2obusan.com
Authorization: Bearer {ACCESS_TOKEN}
Content-type: application/x-www-form-urlencoded;charset=utf-8
```

#### Header

| Name          | Description                                                  | Required |
| ------------- | ------------------------------------------------------------ | -------- |
| Authorization | Application 인증 수단, Application Access Token 값<br />Authorization: Bearer {ACCESS_TOKEN} | O        |

##### Path Variable

| Name      | Type     | Description          | Required |
| :-------- | :------- | :------------------- | :------- |
| USER_UUID | `String` | 조회하려는 사용자 ID | O        |



## Response

| Name      | Type     | Length | Description                                 |
| :-------- | :------- | ------ | :------------------------------------------ |
| uuid      | `String` | 40     | 회원 식별 id                                |
| id        | `String` | 0      | 회원 로그인 id (제공되지 않음.) 고정값 null |
| username  | `String` | 10     | 사용자 이름                                 |
| email     | `String` | 100    | 이메일주소                                  |
| çellphone | `String` | 13     | 휴대폰번호(010-1234-1234)                   |



## Sample

#### Request

``` shell
curl -v -X GET http://bastion.o2obusan.com/v1/profile/hong \
  -H "Authorization: Bearer {APPICATION_ACCESS_TOKEN}"
```

#### Response

```json
HTTP/1.1 200 OK
{
    "uuid": "53w",
    "id": null,
    "username": "관리자",
    "email": "hong",
    "cellphone": "010-1234-0000"
}
```



## Sample source

### Java

```java
String userId = "hong";
HttpHeaders headers = new HttpHeaders();
headers.add("Authorization", String.format("Bearer %s", token));

HttpEntity httpEntity = new HttpEntity(null, headers);

RestTemplate restTemplate = new RestTemplate();
ResponseEntity<ProfileBaseDto> result = restTemplate.exchange("http://bastion.o2obusan.com/v1/profile/"+userId, HttpMethod.GET, httpEntity, ProfileBaseDto.class);

if(result.getBody() == null){
    return;
}
//사용자 정보 객체 조회
ProfileBaseDto resultProfileMe = result.getBody();
```

