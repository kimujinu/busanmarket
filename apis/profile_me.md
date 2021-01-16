# 사용자 정보 반환 API

현재 로그인한 사용자의 정보를 불러옵니다. 사용자 정보 요청 REST API는 사용자 액세스 토큰을 사용하는 방법, 앱 어드민 키를 사용하는 방법 두 가지로 제공됩니다. 어드민 키는 보안에 유의하여 사용해야 하므로 서버에서 호출할 때만 사용합니다.

사용자 액세스 토큰 또는 어드민 키와 회원번호를 헤더(Header)에 담아 `GET`으로 요청합니다. 

# 사용자 정보조회 API (User Token 이용)

## Reequest

#### URL

```html
GET /v1/profile/me HTTP/1.1
Host: bastion.devopscloudlab.com
Authorization: Bearer {ACCESS_TOKEN}
Content-type: application/x-www-form-urlencoded;charset=utf-8
```

#### Header

| Name          | Description                                                  | Required |
| ------------- | ------------------------------------------------------------ | -------- |
| Authorization | 사용자 인증 수단, 액세스 토큰 값<br />Authorization: Bearer {ACCESS_TOKEN} | O        |



## Response

| Name     | Type     | Description    |
| :------- | :------- | :------------- |
| uuid     | `String` | 회원 식별 id   |
| id       | `String` | 회원 로그인 id |
| username | `String` | 사용자 이름    |



## Sample

#### Request

``` shell
curl -v -X GET https://bastion.devopscloudlab.com/v1/profile/me \
  -H "Authorization: Bearer {ACCESS_TOKEN}"
```

#### Response

```json
HTTP/1.1 200 OK
{
    "uuid": "b77eafed-69ab-422d-8448-1ec1f0a2eb8c",
    "id" : "hong"
    "username": "홍길동",
}
```



## Sample source

### Java

```java
HttpHeaders headers = new HttpHeaders();
headers.add("Authorization", String.format("Bearer %s", token));

HttpEntity httpEntity = new HttpEntity(null, headers);

RestTemplate restTemplate = new RestTemplate();
ResponseEntity<ProfileBaseDto> result = restTemplate.exchange("https://gapi.gsitm.com/v1/profile/me", HttpMethod.GET, httpEntity, ProfileBaseDto.class);

if(result.getBody() == null){
    return;
}
//사용자 정보 객체 조회
ProfileBaseDto resultProfileMe = result.getBody();
```



# 사용자 정보조회 API (Basic Token 이용)

## Reequest

#### URL

```http
GET /v1/profile/{USER_ID} HTTP/1.1
Host: bastion.devopscloudlab.com
Authorization: Basic {SECRET_KEY}
Content-type: application/x-www-form-urlencoded;charset=utf-8
```

#### Header

| Name          | Description                                                  | Required |
| ------------- | ------------------------------------------------------------ | -------- |
| Authorization | SECRET_KEY = client_id:secret 을 Base64로 인코딩한 값<br />Authorization: Basic {SECRET_KEY} | O        |

##### Path Variable

| Name    | Type     | Description          | Required |
| :------ | :------- | :------------------- | :------- |
| USER_ID | `String` | 조회하려는 사용자 ID | O        |



## Response

| Name     | Type     | Description    |
| :------- | :------- | :------------- |
| uuid     | `String` | 회원 식별 id   |
| id       | `String` | 회원 로그인 id |
| username | `String` | 사용자 이름    |



## Sample

#### Request

``` shell
curl -v -X GET https://bastion.devopscloudlab.com/v1/profile/hong \
  -H "Authorization: Basic {SECRET_KEY}"
```

#### Response

```json
HTTP/1.1 200 OK
{
    "uuid": "b77eafed-69ab-422d-8448-1ec1f0a2eb8c",
    "id" : "hong"
    "username": "홍길동",
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
ResponseEntity<ProfileBaseDto> result = restTemplate.exchange("https://gapi.gsitm.com/v1/profile/"+userId, HttpMethod.GET, httpEntity, ProfileBaseDto.class);

if(result.getBody() == null){
    return;
}
//사용자 정보 객체 조회
ProfileBaseDto resultProfileMe = result.getBody();
```

