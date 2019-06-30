Spring Boot와 JSon Web Token(JWT), Single Sign On(SSO) 

JWT 기반의 SSO는 데이터베이스에 접근하지 않고도 유저를 인증할 수 있다. 

JWT는 Cookie와 Session의 대안으로 만들어진 정보 교환 방식으로 "크로스 도메인 쿠키 문제"에 대안으로 사용될 수 있다. 

즉 Cookie같은 경우에는 발행한 해당 서버에서만 유효하지만, 토큰은 HTML Body형태로 전송하기 때문에 다른 도메인에서도 사용할 수 있다. 



Json Web Token(JWT) : JSON 포맷을 이용한 Web Token으로 기본 구조는 Header, Payload, Signature로 나뉜다.



JSON ex.
header 



payload



signature = Signature는 header와 playload를 base64로 인코딩 후 합쳐진다. 





JWT ex.
token = encodeBase64Url(header) + '.' + encodeBase64Url(payload) + '.' + encodeBase64Url(signature) 
-> eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJsb2dnZWRJbkFzIjoiYWRtaW4iLCJpYXQiOjE0MjI3Nzk2Mzh9.gzSraSYS8EXBxLN_oWnFSRgCzcmJmMjLiuyu5CSpyHI
JWT는 마침표(.)를 구분자로 이용하여 header,payload,signature를 합친다.

\- Claim based Token : Claim이란 사용자에 대한 속성을 의미하는데, JWT는 이 Claim을 JSON값으로 정의한다.





