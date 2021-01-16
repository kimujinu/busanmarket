# 사용자 정보 반환 API

현재 로그인한 사용자의 정보를 불러옵니다. 사용자 정보 요청 REST API는 사용자 액세스 토큰을 사용하는 방법, 앱 어드민 키를 사용하는 방법 두 가지로 제공됩니다. 어드민 키는 보안에 유의하여 사용해야 하므로 서버에서 호출할 때만 사용합니다.

사용자 액세스 토큰 또는 어드민 키와 회원번호를 헤더(Header)에 담아 `GET` 또는 `POST`로 요청합니다. 어드민 키로 요청할 때는 어떤 사용자의 정보가 필요한지 명시하기 위해 대상 사용자의 ID를 전달합니다. 추가 파라미터를 사용하면 특정 정보만 지정해서 받아오거나 URL 응답 값을 `HTTPS`로 받을지 지정할 수 있습니다.

## Reequest

#### URL

```shell
GET/POST /v1/profile/me HTTP/1.1
Host: gapi.gsitm.com
Authorization: Bearer {ACCESS_TOKEN}
Content-type: application/x-www-form-urlencoded;charset=utf-8
```

#### Header

| Name          | Description                                                  | Required |
| ------------- | ------------------------------------------------------------ | -------- |
| Authorization | 사용자 인증 수단, 액세스 토큰 값<br />Authorization: Bearer {ACCESS_TOKEN} | O        |



## Response

### Sample

##### Request : Access Token, 모든 사용자 정보 조회

``` shell
curl -v -X GET https://gapi.gsitm.com/v1/profile/me \
  -H "Authorization: Bearer {ACCESS_TOKEN}"
```

##### Request: Access Token, email 정보 받기

```shell
curl -v -X POST https://gapi.gsitm.com/v1/profile/me \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -d 'property_keys=["kakao_account.email"]'
```

##### Response: 성공

```json
HTTP/1.1 200 OK
{
  "id":123456789,
  "profile": {
      "nickname": "홍길동",
      "thumbnail_image_url": "http://yyy.gsitm.com/.../img_110x110.jpg",
      "profile_image_url": "http://yyy.gsitm.com/dn/.../img_640x640.jpg"
  }
}
```

### 

#### 프로필(profile)

| Name                | Type     | Description                                                  |
| :------------------ | :------- | :----------------------------------------------------------- |
| nickname            | `String` | 닉네임                                                       |
| profile_image       | `String` | 프로필 이미지 URL, 640px * 640px 또는 480px * 480px          |
| thumbnail_image_url | `String` | 프로필 미리보기 이미지 URL, 110px * 110px 또는 100px * 100px |





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



### Nodejs

```javascript
class UserService {
    getMe() {
        return axios.get(process.env.VUE_APP_USER_SERVICE_URL+'/profile/me')
        .then(res => {
            const me = res.data;
            return Promise.resolve(new User(me.empNo, me.email, me.displayName, me.department));
        });
    }
}
 
export default new UserService()
```