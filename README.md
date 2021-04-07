# busanmarket



author : Dennis (dennis@gsitm.com)
since : 2021-01-16

# API Http응답, 반환코드, 메시지 가이드

1. Http Status Code가 200일 경우만 정상처리 된 것으로 판단합니다.
2. Http Status Code가 400일 경우 정상적으로 처리되지 않는 것이며, body에 resultCode와 resultMessage로 자세한 내용을 확인 할수 있음

# API List

## 로그인관련 API

[1. 로그인 API](./apis/authentication.md)<br/>
[2. Token 관련 API](./apis/token.md)<br/>
[3, 사용자 정보 API](./apis/profile_me.md)

## 통합 API

[1. 주소관련 API](./apis/address.md)<br/>[2. 결제관련 API](./apis/payment.md)