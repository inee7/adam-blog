# JWT를 구현하면서 마주치게 되는 고민들

03 MAR 2017 on [Develop](https://swalloow.github.io/tag/Develop)

 

최근 모바일, 웹 등 다양한 환경에서 서버와 통신하면서 많은 사람들이 JWT 토큰 인증 방식을 추천합니다. 이 포스팅에서는 JWT를 이해하고 구현하면서 마주치게 되는 고민들에 대해 정리해보려 합니다.

 

## JSON Web Token

![JWT-Token](https://cdn.auth0.com/blog/legacy-app-auth/legacy-app-auth-5.png)

JWT에 대한 소개는 생략하고 Token이 어떻게 구성되어 있는지 간략하게 알아보겠습니다. JSON Web Token은 세 파트로 나뉘어지며, 각 파트는 점(.)에 의해 구분됩니다. 이를 테면 `xxxxx.yyyyy.zzzzz` 이런식입니다.

- Header는 토큰의 타입과 해시 암호화 알고리즘으로 구성되어 있습니다.
- Payload는 claim 정보를 포함하고 있습니다. userId, expire, scope 등이 여기에 해당합니다.
- 마지막으로 Signature는 secret key를 포함하여 암호화되어 있습니다.

 

## JWT Process

![JWT-Diagram](https://cdn.auth0.com/content/jwt/jwt-diagram.png)

일반적으로 JWT 토큰 기반의 인증 시스템은 위와 같은 프로세스로 이루어집니다. 처음 사용자를 등록할 때 Access token과 Refresh token이 모두 발급되어야 합니다.

1. 먼저 사용자가 id와 password를 입력하여 로그인을 시도합니다.
2. 서버는 요청을 확인하고 secret key를 통해 Access token을 발급합니다.
3. 이후 JWT가 요구되는 API를 요청할 때는 클라이언트가 Authorization header에 Access token을 담아서 보냅니다.
4. 서버는 JWT Signature를 체크하고 Payload로부터 user 정보를 확인해 데이터를 리턴합니다.

 

## JWT와 기존의 OAuth는 서로 어떤 관계가 있을까?

토큰 기반의 인증 시스템을 처음 접한 사람이라면 저 두 가지 개념이 헷갈릴 수 있습니다. 먼저, 정답부터 말하자면 JWT는 토큰 유형이고 OAuth는 토큰을 발급하고 인증하는 방법을 설명하는 일종의 프레임워크입니다. 기존의 /outh/token endpoint에 의해 발급되는 모든 토큰은 일종의 OAuth 프레임워크에 의해 관리된다고 볼 수 있습니다.

```
{
	"token_type":"bearer",
	"access_token":"eyJ0eXAiOiJKV1QiLCJh",
	"expires_in":20,
	"refresh_token":"fdb8fdbecf1d03ce5e6125c067733c0d51de209c"
}
```

위의 토큰이 기존 OAuth에서 주로 사용하는 bearer 기반의 토큰 방식입니다. 다만 JWT는 토큰 자체에 유저 정보를 담아서 HTTP 헤더로 전달하기 때문에 유저 세션을 유지할 필요가 없고 가볍게 데이터를 주고받을 수 있다는 장점이 있습니다.

 

## Access Token과 Refresh Token을 어디에 저장해야 할까?

앞서 말한것처럼 기본적으로 두 가지 토큰을 사용합니다. API 요청을 허가하는데 Access Token을 사용하고, 액세스 토큰이 만료된 후 새로운 액세스 토큰을 얻기 위해 Refresh Token을 사용합니다.

![access_token](https://swalloow.github.io/assets/images/access%20token.png)

Access Token은 리소스에 직접 접근할 수 있도록 해주는 정보만을 가지고 있습니다. 즉, 클라이언트는 Access Token이 있어야 서버 자원에 접근할 수 있습니다. Access Token은 짧은 수명을 가지며, 만료기간을 갖습니다. 주로 **세션** 에 담아서 관리합니다.

![refresh_token](https://swalloow.github.io/assets/images/refresh%20token.png)

Refresh Token은 새로운 Access Token을 발급받기 위한 정보를 갖습니다. 즉, 클라이언트가 Access Token이 없거나 만료되었다면 Refresh Token을 통해 Auth Server에 요청해서 발급받을 수 있습니다. Refresh Token 또한 만료기간이 있지만 깁니다. Refresh Token은 중요하기 때문에 외부에 노출되지 않도록 엄격하게 관리해야 하므로 주로 **데이터베이스** 에 저장합니다.

토큰이 안전하게 관리되는지 여부는 어떻게 구현하느냐에 달려있습니다. 보통은 Access Token에 대해 직접적으로 인증(direct authorization) 체크합니다. 무슨 말이냐 하면, Access Token이 서버 자원에 접근하려고 하면 서버가 토큰에 있는 정보를 읽어 스스로 인증여부를 결정합니다. 반면에, Refresh Token은 Auth Server에 대한 체크가 필요합니다. 이때 다음과 같은 세 가지 사항을 고려해야 합니다.

- 빠른 인증 과정을 위해 토큰에 정보를 적게 담아야 합니다.
- 노출된 Access Token에 대해서는 빠르게 만료시켜 리소스에 접근할 수 있는 가능성을 줄어야 합니다.
- Sliding sessions에 대한 처리가 필요합니다.

 

#### Sliding sessions

Sliding sessions은 일정 기간 사용하지 않으면 만료되는 세션입니다. 이 세션은 refresh token과 access token을 통해 구현할 수 있습니다. 먼저, 사용자가 작업을 수행하려하면 새 access token이 발급됩니다. 반면, 사용자가 만료된 access token을 사용하려 하면 세션은 비활성화되며 새 access token을 요청합니다. 이 상황에서 refresh token을 통해 새로운 토큰을 발급받을 지는 개발 팀이 결정하기에 따라 다릅니다.

 

## 토큰 재발급 로직에 대한 고민

JWT를 쓴다면 만료시간을 꼭 명시적으로 두도록 하고 중간마다 토큰을 재발행하도록 권장하는 것이 대부분입니다. 리프레시 관련해서 정확한 내용이 정해져 있는 것은 아니지만 일반적인 API를 보면 최초 발급시 Access Token과 Refresh Token 2개를 발급하고 Access Token으로 API를 사용하다가 만료시간이 지나면 만료시간을 길게 준 Refresh Token을 이용해서 Access Token을 다시 발급합니다.

가장 일반적인 방법은 클라이언트가 토큰의 만료시간을 알 수 있기 때문에, 클라이언트에서 판단해서 만료시간이 넘었으면 토큰 재발급을 요청하는 방법입니다. 좀 더 쉽게 구현하는 방법은 TokenExpiredError가 발생했을 때 재발급을 해주는 것입니다.

만료된 토큰을 통해 접근하려고 하면 다음과 같은 오류가 나타나게 합니다.

```
{
	"code":401,
	"error":"invalid_token",
	"error_description":"The access token provided has expired."
}
```

 

## 기타, 참고자료

Flask에는 Flask-JWT라는 패키지가 있지만 Scope나 Refresh Token에 대한 구현이 부족하기 때문에 pyjwt를 통해 직접 구현하는 방법을 추천합니다.

아래는 JWT에 대하여 가장 많이 도움이 되는 페이지입니다. 들어가서 가입하면 **JWT Handbook** 이라는 EBook도 무료로 제공합니다. https://jwt.io/

- [10 things you should know about tokens and cookies](https://auth0.com/blog/ten-things-you-should-know-about-tokens-and-cookies/)
- [Learn refresh tokens](https://auth0.com/learn/refresh-tokens/)
- [How to identify if the OAuth token has expired](http://stackoverflow.com/questions/30826726/how-to-identify-if-the-oauth-token-has-expired)