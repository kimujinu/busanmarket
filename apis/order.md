# Order API

인증 코드로 액세스 토큰과 리프레시 토큰을 발급받는 API입니다. 인증코드 발급이후 토큰발급을 완료하여야 로그인을 완료한 것입니다,

필수 파라미터 값들을 담아 *POST*로 요청합니다. 요청 성공 시, 응답은 `JSON` 객체로 `Redirect URI`에 전달되며 두 가지 종류의 토큰 값과 타입, 초 단위로 된 만료 시간을 포함하고 있습니다.

사용자가 로그인에 성공하면 발급되는 액세스 토큰(Access Token)과 리프레시 토큰(Refresh Token)은 각각 역할과 유효기간이 다릅니다. 실제 사용자 인증을 맡는 액세스 토큰은 비교적 짧은 만료 시간을 가집니다. 하지만 유효한 리프레시 토큰이 있다면, 사용자가 다시 로그인했을 때 리프레시 토큰으로 액세스 토큰을 다시 발급받을 수 있습니다. 



## 목차

[1.주문 등록 API](#주문-등록-API)</br>
[2.주문상태 변경 API](#주문상태-변경-API)</br>

# 주문 등록 API

## Request

##### URL

```http
POST /v1/order HTTP/1.1
Host: bastion.o2obusan.com
Content-type: application/x-www-form-urlencoded;charset=utf-8
```

##### Header

| Name           | Type     | Description              | Required |
| :------------- | :------- | :----------------------- | :------- |
| appId          | `String` | 인증서버에서 발급받은 Id | O        |
| Authentication | `String` | 사용자 토큰              | O        |



##### Parameter

| Name           | Type      | Description                                               | Required |
| :------------- | :-------- | :-------------------------------------------------------- | :------- |
| storeId        | `String`  | 상점ID(가맹점번호)                                        | O        |
| storeName      | `String`  | 상점이름                                                  | O        |
| orderNo        | `String`  | 주문번호                                                  | O        |
| orderPrice     | `Integer` | 주문금액                                                  | O        |
| orderTitleText | `String`  | 주문타이틀 (예: 반반치킨외 1개 23,500원)                  | O        |
| orderSubText   | `String`  | 주문보조타이틀 (옵션 텍스트 표시, 예: 콜라외 2개 5,000원) |          |
| status         | `String`  | 주문상태 Text (주문완료, 주문접수등등 텍스트로 전송)      | O        |
| imageUrl       | `String`  | 대표이미지 URL                                            |          |
| storeUrl       | `String`  | 상점페이지이동 URL                                        |          |
| detailUrl      | `String`  | 주문 상세페이지 URL                                       | O        |
| deliveryUrl    | `String`  | 배송상세페이지 URL                                        |          |
| orderDateTime  | `String`  | 주문일시 (yyyyMMddHH24miss)                               | O        |



## Response

| Name      | Type     | Description  |
| :-------- | :------- | :----------- |
| orderUuid | `String` | 공통주문번호 |



## Sample

### Request

```shell
curl -v -X POST http://bastion.o2obusan.com/v1/order \
-d 'storeId={storeId}' \
-d 'storeName={storeName}' \
-d 'orderNo={orderNo}' \
-d 'orderPrice={orderPrice}' \
-d 'orderTitleText={orderTitleText}' \
-d 'orderSubText={orderSubText}' \
-d 'status={status}' \
-d 'imageUrl={imageUrl}' \
-d 'storeUrl={storeUrl}' \
-d 'detailUrl={detailUrl}' \
-d 'deliveryUrl={deliveryUrl}' \
-d 'orderDateTime={orderDateTime}'
```



### Response

```java
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{"orderUuid":"eyJhbGci3Bo"}
```



### Sample Code

#### Java

```java
String clientId = "dev-shop";
MultiValueMap<String, String> body = new LinkedMultiValueMap<>();
body.add("storeId", "s20000");
body.add("storeName", "스시산");
body.add("orderNo", "xdftdf238drsf");
body.add("orderPrice", "20000");
body.add("orderTitleText", "오늘의회(소)");
body.add("orderSubText", "매운탕추가(2,000원)");
body.add("status", "주문접수대기");
body.add("imageUrl", "https://busancdn.o2obusan.com/images/duy3nb239.jpg");
body.add("storeUrl", "https://market.o2obusan.com/s20000");
body.add("detailUrl", "https://market.o2obusan.com/orders?orderId=xdftdf238drsf");
body.add("deliveryUrl", "https://market.o2obusan.com/delivery?orderId=xdftdf238drsf");
body.add("orderDateTime", "20210128173028");

HttpHeaders headers = new HttpHeaders();
headers.add("Authorization", String.format("Bearer %s", token));
headers.add("appId", clientId)

HttpEntity httpEntity = new HttpEntity(body, headers);
RestTemplate restTemplate = new RestTemplate();

ResponseEntity<GatewayOrder> result = restTemplate.postForEntity("/v1/order", httpEntity, GatewayOrder.class);
```



# 주문 상태변경 API

## Request

##### URL

```http
PUT /v1/order HTTP/1.1
Host: bastion.o2obusan.com
Content-type: application/x-www-form-urlencoded;charset=utf-8
```

##### Header

| Name           | Type     | Description              | Required |
| :------------- | :------- | :----------------------- | :------- |
| appId          | `String` | 인증서버에서 발급받은 Id | O        |
| Authentication | `String` | 사용자 토큰              | O        |



##### Parameter

| Name      | Type     | Description                                          | Required |
| :-------- | :------- | :--------------------------------------------------- | :------- |
| orderUuid | `String` | 공통주문번호                                         | O        |
| status    | `String` | 주문상태 Text (주문완료, 주문접수등등 텍스트로 전송) | O        |




## Response

| Name      | Type     | Description  |
| :-------- | :------- | :----------- |
| orderUuid | `String` | 공통주문번호 |



## Sample

### Request

```shell
curl -v -X POST http://bastion.o2obusan.com/v1/order \
-d 'orderUuid={orderUuid}' \
-d 'status={status}'
```



### Response

```java
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{"orderUuid":"eyJhbGci3Bo", "status":"배송시작"}
```



### Sample Code

#### Java

```java
String clientId = "dev-shop";
MultiValueMap<String, String> body = new LinkedMultiValueMap<>();
body.add("orderUuid", "eyJhbGci3Bo");
body.add("status", "배송시작");

HttpHeaders headers = new HttpHeaders();
headers.add("Authorization", String.format("Bearer %s", token));
headers.add("appId", clientId)

HttpEntity httpEntity = new HttpEntity(body, headers);
RestTemplate restTemplate = new RestTemplate();

ResponseEntity<GatewayOrder> result = restTemplate.putForEntity("/v1/order", httpEntity, GatewayOrder.class);
```

