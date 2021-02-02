# Token API

인증 코드로 액세스 토큰과 리프레시 토큰을 발급받는 API입니다. 인증코드 발급이후 토큰발급을 완료하여야 로그인을 완료한 것입니다,

필수 파라미터 값들을 담아 *POST*로 요청합니다. 요청 성공 시, 응답은 `JSON` 객체로 `Redirect URI`에 전달되며 두 가지 종류의 토큰 값과 타입, 초 단위로 된 만료 시간을 포함하고 있습니다.

사용자가 로그인에 성공하면 발급되는 액세스 토큰(Access Token)과 리프레시 토큰(Refresh Token)은 각각 역할과 유효기간이 다릅니다. 실제 사용자 인증을 맡는 액세스 토큰은 비교적 짧은 만료 시간을 가집니다. 하지만 유효한 리프레시 토큰이 있다면, 사용자가 다시 로그인했을 때 리프레시 토큰으로 액세스 토큰을 다시 발급받을 수 있습니다. 



## 목차

[1.User Access Token 발급 API](#User-Access-Token-발급-API)</br>
[2.Application Access Token 발급 API](#Application-Access-Token-발급-API)</br>
[3.Token 검증 API](#Token-검증-API)</br>
[4.Token 갱신 API](#Token-갱신-API)</br>
[5.Token 만료 API](#Token-만료-API)</br>

# User Access Token 발급 API

## Request

##### URL

```http
POST /v1/oauth/token HTTP/1.1
Host: bastion.o2obusan.com
Content-type: application/x-www-form-urlencoded;charset=utf-8
```

##### Parameter

| Name         | Type     | Description                     | Required |
| :----------- | :------- | :------------------------------ | :------- |
| grant_type   | `String` | `authorization_code`로 전달     | O        |
| client_id    | `String` | 인증서버에서 받급받은 client_id | O        |
| redirect_uri | `String` | 인증코드 Redirect URI           | O        |
| code         | `String` | 인증코드                        | O        |



## Response

| Name                     | Type      | Description                                                  |
| :----------------------- | :-------- | :----------------------------------------------------------- |
| token_type               | `String`  | 토큰 타입, `bearer`로 고정                                   |
| access_token             | `String`  | 사용자 액세스 토큰 값                                        |
| expires_in               | `Integer` | 액세스 토큰 만료 시간(초)                                    |
| refresh_token            | `String`  | 사용자 리프레시 토큰 값                                      |
| refresh_token_expires_in | `Integer` | 리프레시 토큰 만료 시간(초)                                  |
| scope                    | `String`  | 인증된 사용자의 정보 조회 권한 범위 범위가 여러 개일 경우, 공백으로 구분 |



## Sample

### Request

```shell
curl -v -X POST http://bastion.o2obusan.com/v1/oauth/token \
 -d 'grant_type=authorization_code' \
 -d 'client_id={CLIENT_ID}' \
 -d 'redirect_uri={REDIRECT_URI}' \
 -d 'code={AUTHORIZATION_CODE}'
```



### Response

```java
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{"access_token":"eyJhbGci3Bo","token_type":"bearer","refresh_token":"eyJhbGciKZW1c5v_Yw","expires_in":35998,"scope":"read","uuid":"53w","jti":"35084cb6-9ae2-48f3-bacd-d6fa8e10b658"}
```



### Sample Code

#### Java

```java
MultiValueMap<String, String> body = new LinkedMultiValueMap<>();
body.add("grant_type", "authorization_code");
body.add("redirect_uri", resourceProperties.getRedirectUri());
body.add("scope", "read");
body.add("code", code);

HttpHeaders headers = new HttpHeaders();
headers.add("Authorization",
            String.format("Basic %s", new String(Base64.encode("client:secret".getBytes()))));

HttpEntity httpEntity = new HttpEntity(body, headers);
RestTemplate restTemplate = new RestTemplate();

ResponseEntity<OAuth2AccessToken> result = restTemplate.postForEntity(resourceProperties.getAccessTokenUri(), httpEntity, OAuth2AccessToken.class);
OAuth2AccessToken token = result.getBody();
```



# Application Access Token 발급 API

## Request

##### URL

```http
POST /v1/oauth/token HTTP/1.1
Host: bastion.o2obusan.com
Content-type: application/x-www-form-urlencoded;charset=utf-8
```

##### Parameter

| Name       | Type     | Description                 | Required |
| :--------- | :------- | :-------------------------- | :------- |
| grant_type | `String` | `client_credentials`로 전달 | O        |
| scope      | `String` | `client`로 전달             | O        |



## Response

| Name         | Type      | Description                                                  |
| :----------- | :-------- | :----------------------------------------------------------- |
| token_type   | `String`  | 토큰 타입, `bearer`로 고정                                   |
| access_token | `String`  | 어플리케이션 액세스 토큰 값                                  |
| expires_in   | `Integer` | 액세스 토큰 만료 시간(초)                                    |
| scope        | `String`  | 인증된 어플리케이션의 정보 조회 권한 범위 범위가 여러 개일 경우, 공백으로 구분 |



## Sample

### Request

```shell
curl -v -X POST http://bastion.o2obusan.com/v1/oauth/token \
 -d 'grant_type=client_credentials' \
 -d 'scope=client'
```



### Response

```java
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{"access_token":"eyJhbGci3Bo","token_type":"bearer","expires_in":35998,"scope":"client","jti":"35084cb6-9ae2-48f3-bacd-d6fa8e10b658"}
```



### Sample Code

#### Java

```java
MultiValueMap<String, String> body = new LinkedMultiValueMap<>();
body.add("grant_type", "client_credentials");
body.add("scope", "client");

HttpHeaders headers = new HttpHeaders();
headers.add("Authorization",
            String.format("Basic %s", new String(Base64.encode("client:secret".getBytes()))));

HttpEntity httpEntity = new HttpEntity(body, headers);
RestTemplate restTemplate = new RestTemplate();

ResponseEntity<OAuth2AccessToken> result = restTemplate.postForEntity(resourceProperties.getAccessTokenUri(), httpEntity, OAuth2AccessToken.class);
OAuth2AccessToken token = result.getBody();
```



# Token 검증 API

## Request

##### URL

```http
POST /v1/oauth/token HTTP/1.1
Host: bastion.o2obusan.com
Content-type: application/x-www-form-urlencoded;charset=utf-8
```

##### Parameter

| Name         | Type     | Description                     | Required |
| :----------- | :------- | :------------------------------ | :------- |
| grant_type   | `String` | `authorization_code`로 전달     | O        |
| client_id    | `String` | 인증서버에서 받급받은 client_id | O        |
| redirect_uri | `String` | 인증코드 Redirect URI           | O        |
| code         | `String` | 인증코드                        | O        |
|              |          |                                 |          |



## Response

| Name                     | Type      | Description                                                  |
| :----------------------- | :-------- | :----------------------------------------------------------- |
| token_type               | `String`  | 토큰 타입, `bearer`로 고정                                   |
| access_token             | `String`  | 사용자 액세스 토큰 값                                        |
| expires_in               | `Integer` | 액세스 토큰 만료 시간(초)                                    |
| refresh_token            | `String`  | 사용자 리프레시 토큰 값                                      |
| refresh_token_expires_in | `Integer` | 리프레시 토큰 만료 시간(초)                                  |
| scope                    | `String`  | 인증된 사용자의 정보 조회 권한 범위 범위가 여러 개일 경우, 공백으로 구분 |



## Sample

### Request

```shell
curl -v -X POST http://bastion.o2obusan.com/v1/oauth/token \
 -d 'grant_type=authorization_code' \
 -d 'client_id={CLIENT_ID}' \
 -d 'redirect_uri={REDIRECT_URI}' \
 -d 'code={AUTHORIZATION_CODE}'
```



### Response

```java
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{"access_token":"eyJhbGci3Bo","token_type":"bearer","refresh_token":"eyJhbGciKZW1c5v_Yw","expires_in":35998,"scope":"read","uuid":"53w","jti":"35084cb6-9ae2-48f3-bacd-d6fa8e10b658"}
```



### Sample Code

#### Java

```java
MultiValueMap<String, String> body = new LinkedMultiValueMap<>();
body.add("grant_type", "authorization_code");
body.add("redirect_uri", resourceProperties.getRedirectUri());
body.add("scope", "read");
body.add("code", code);

HttpHeaders headers = new HttpHeaders();
headers.add("Authorization",
            String.format("Basic %s", new String(Base64.encode("client:secret".getBytes()))));

HttpEntity httpEntity = new HttpEntity(body, headers);
RestTemplate restTemplate = new RestTemplate();

ResponseEntity<OAuth2AccessToken> result = restTemplate.postForEntity(resourceProperties.getAccessTokenUri(), httpEntity, OAuth2AccessToken.class);
OAuth2AccessToken token = result.getBody();
```



# Token 갱신 API

## Request

##### URL

```http
POST /v1/oauth/token HTTP/1.1
Host: bastion.o2obusan.com
Authorization: Basic {SECRET_KEY}
Content-type: application/x-www-form-urlencoded;charset=utf-8
```

#### Header

| Name          | Description                                                  | Required |
| ------------- | ------------------------------------------------------------ | -------- |
| Authorization | SECRET_KEY = client_id:secret 을 Base64로 인코딩한 값<br />Authorization: Basic {SECRET_KEY} | O        |

##### Parameter

| Name          | Type     | Description                     | Required |
| :------------ | :------- | :------------------------------ | :------- |
| grant_type    | `String` | `refresh_token`로 전달          | O        |
| refresh_token | `String` | 인증서버에서 받급받은 client_id | O        |



## Response

| Name                     | Type      | Description                                                  |
| :----------------------- | :-------- | :----------------------------------------------------------- |
| token_type               | `String`  | 토큰 타입, `bearer`로 고정                                   |
| access_token             | `String`  | 사용자 액세스 토큰 값                                        |
| expires_in               | `Integer` | 액세스 토큰 만료 시간(초)                                    |
| refresh_token            | `String`  | 사용자 리프레시 토큰 값                                      |
| refresh_token_expires_in | `Integer` | 리프레시 토큰 만료 시간(초)                                  |
| scope                    | `String`  | 인증된 사용자의 정보 조회 권한 범위 범위가 여러 개일 경우, 공백으로 구분 |



## Sample

### Request

```shell
curl -v -X POST http://bastion.o2obusan.com/v1/oauth/token \
 -d 'grant_type=refresh_token' \
 -d 'refresh_token={REFRESH_TOKEN}'
```



### Response

```java
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{"access_token":"eyJhbGci3Bo","token_type":"bearer","refresh_token":"eyJhbGciKZW1c5v_Yw","expires_in":35998,"scope":"read","uuid":"53w","jti":"35084cb6-9ae2-48f3-bacd-d6fa8e10b658"}
```



### Sample Code

#### Java

```java
MultiValueMap<String, String> body = new LinkedMultiValueMap<>();
body.add("grant_type", "refresh_token");
body.add("refresh_token", resourceProperties.getRedirectUri());

HttpHeaders headers = new HttpHeaders();
headers.add("Authorization",
            String.format("Basic %s", new String(Base64.encode("client:secret".getBytes()))));

HttpEntity httpEntity = new HttpEntity(body, headers);
RestTemplate restTemplate = new RestTemplate();

ResponseEntity<OAuth2AccessToken> result = restTemplate.postForEntity(resourceProperties.getAccessTokenUri(), httpEntity, OAuth2AccessToken.class);
OAuth2AccessToken token = result.getBody();
```





# Token 만료 API

## Request

##### URL

```http
DELETE /v1/oauth/revoke HTTP/1.1
Host: bastion.o2obusan.com
Content-type: application/x-www-form-urlencoded;charset=utf-8
```

#### 

### Parameter

| Name         | Type     | Description                     | Required |
| :----------- | :------- | :------------------------------ | :------- |
| client_id    | `String` | 인증서버에서 받급받은 client_id | O        |
| revoke_token | `String` | 만료대상토큰                    |          |



## Response

| Name         | Type     | Description |
| :----------- | :------- | :---------- |
| revoke_token | `String` | 만료토큰    |

## Sample

### Request

```shell
curl -v -X DELETE http://bastion.o2obusan.com/v1/oauth/revoke \
 -d 'client_id={CLIENT_ID}' \
 -d 'revoke_token={REVOKE_TOKEN}' \
```



### Response

```java
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{"revoke_token":"eyJhbGci3Bo"}
```



### Sample Code

#### Java

```java
MultiValueMap<String, String> body = new LinkedMultiValueMap<>();
body.add("client_id", resourceProperties.getClientId());
body.add("revoke_token", token);

HttpEntity httpEntity = new HttpEntity(body);
RestTemplate restTemplate = new RestTemplate();

ResponseEntity<OAuth2AccessToken> result = restTemplate.deleteForEntity(resourceProperties.getRevokeTokenUri(), httpEntity, OAuth2AccessToken.class);
OAuth2AccessToken token = result.getBody();
```



