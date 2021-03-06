---  
layout: post  
title:  OAuth 2.0 Authorization Framework 표준
date:  2019-05-21 00:00:00 +0900  
categories: [Spring-Boot, OAuth2]
---

스프링에서는 OAuth2를 구성하기 위해 먼저 크게 두가지, OAuth 2.0 provider와 client를 제공한다.

## OAuth 2.0 Provider

접근이 제한되어 있는 리소스(protected resource)를 제공하는 역할. OAuth 2.0에서 유저를 대신해서 제한된 리소스에 엑세스할 수 있는 클라이언트(client) 역할을 수행한다. 이 프로바이더는 OAuth 2.0의 리소스 접근에 사용되는 토큰을 검증하고(verifying) 관리함으로써 이러한 역할을 수행한다. 

### Oauth 2.0 Provider Implementation

OAuth 2.0에서 provider의 역할은 인증 서비스(Authorization Service)와 리소스 서비스(Resource Service)를 분리하는 것이다. 이 분리 구조는 필요에 따라서 같은 애플리케이션에 구축되기도하며 Spring Security OAuth는 여러개의 리소스 서비스가 하나의 인증 서버를 구현하거나 하는 등의  다양한 구성을 지원한다.

토큰 요청은 Spring MVC controller endpoints가 담당하며 접근 제한 리소스는 Standard Spring Security request filter가 담당한다. 인증 서버를 구현하기 위한 Spring Security filter chain에 아래와 같은 엔드포인트가 요구된다.

* `AuthorizationEndpoint` - `default: /oauth/authorize`, 인증을 위한 서비스 리퀘스트에 사용
* `TokenEndpoint` - `default: /oauth/token`, 엑세스 토큰을 위한 서비스 리퀘스트에 사용

다음은 리소스 서버를 구현하기 위해 요구되는 필터

* `OAuth2AuthenticationProcessingFilter` - 리퀘스트와 함께 온 인증 엑세스 토큰의 정보를 로드하고 처리하는데 사용

### Authorization Server Configuration

인증 서버를 구성하기 전에 어떤 승인 방식(Grant Type)을 사용할 지 고려해야 한다. 서버의 환경설정은 `client details service implementation`, `token service implementation`, 전역적으로 어떻게 허용할지 안할지에 대한 구현체를 제공하는데 사용된다. Note, however, that each client can be configured specifically with permissions to be able to use certain authorization mechanisms and access grants. I.e. just because your provider is configured to support the "client credentials" grant type, doesn't mean that a specific client is authorized to use that grant type.

`@EnableAuthorizationServer` 애노테이션은 OAuth 2.0 인증 서버를 설정하는데 사용되는데 `AuthorizationServerConfigurer`를 구현하는(어댑터 구현도 있음) `@Beans`와 함께 사용해야 한다. 

* `ClientDetailsServiceConfigurer` : client details service를 정의하는 설정자(Configurer). Client details는 초기화될 수 있고 또는 이미 만들어져 있는 것을 참조할 수 있음
* `AuthorizationServerSecurityConfigurer` : token endpoint에 대한 보안 제약을 정의
* `AuthorizationServerEndpointsConfigurer` : 인증과 토큰 엔드포인트, 토큰 서비스를 정의

프로바이더 설정에서의 중요한 점은 OAuth client에게 인증 코드를 제공하는 방법을 정의하는 것이다. 인증 코드는 OAuth client가 end-user에게 '인증 서버에서 제공하는 인증 페이지에서 증명서를 이용해 인증을 받아서 나오는 결과물을 나한테 줘'라고 지시함으로써 얻어진다.

### Configuring Client Details
`ClientDetailsServiceConfigurer`(`AuthorizationServerConfigurer`에 의해서 콜백되는)는 in-memory 또는 JDBC 방식을 지원한다. 아래는 주요 속성이다.

*   `clientId`: (required) the client id.
*   `secret`: (required for trusted clients) the client secret, if any.
*   `scope`: The scope to which the client is limited. If scope is undefined or empty (the default) the client is not limited by scope.
*   `authorizedGrantTypes`: Grant types that are authorized for the client to use. Default value is empty.
*   `authorities`: Authorities that are granted to the client (regular Spring Security authorities).

Client details can be updated in a running application by access the underlying store directly (e.g. database tables in the case of `JdbcClientDetailsService`) or through the `ClientDetailsManager` interface (which both implementations of `ClientDetailsService` also implement).

### Managing Tokens

`AuthorizationServerTokenServices` 인터페이스는 OAuth 2.0에서 토큰을 관리하는 필수적인 운영에 대한 구현을 제공함.

* 엑세스 토큰이 생성될 때 리소스를 허용하는 엑세스 토큰을 나중에 참조할 수 있도록 반드시 인증을 저정해야 함.
* 엑세스 토큰은 인증을을 로드하는데 사용됨

`AuthorizationServerTokenServices` 구현체가 구현되면 엑세스 토큰을 저장하거나 그 양식을 변경하는데 얼마든지 유연하게 적용 가능한 많은 전략들을 설정할 수 있다. 기본적으로 토큰은 랜덤값을 기반으로 만들어지며 토큰의 영구 저장(Persistence)와 관련된 객체인 `TokenStore`이 하는 일을 제외하고 토큰을 다루는 거의 모든 일들을 한다고 보면 된다. store의 기본 구현체는 `in-memory implementation`이지만 변경가능하다.

* `InMemoryTokenStore` - 싱글 서버에 사용하기 적합(트래픽이 적고 failure을 대비한 backup 서버로의 hot swap이 필요하지 않음). 대부분의 프로젝트가 이 구현체로 시작하며 개발 모드에서도 많이 사용된다. 별도의 디펜던시가 필요하지 않아서 서버를 구성하기 쉽다.
* `JdbcTokenStore` - 토큰 데이터를 관계형 데이터에 저장. 서버간에 데이터베이스를 사용하여 공유.
* `JSON Web Token(JWT)` - 토큰에 권한과 관련된 데이터를 넣고 그 자체를 인코딩하여 저장(백 엔드 저장소가 없는 장점이 있음). 단점은 토큰을 취소하기가 어려움. 이 방법을 보완하기 위하여 유효시간을 짧게 주고 refresh token을 사용하여 재발급 받게 하는 방법이 있음. 다른 단점으로는 너무 많은 양의 정보가 토큰에 담기면 낭비가 될 수 있음.(계속 주고 받아야 하므로). `JwtTokenStore`는 실제로는 'store'는 아니며 어떤 데이터도 영구(persist) 저장하지 않음. 하지만 토큰 문자열과 인증 정보를 변환하는데 `DefaultTokenServices`를 사용함.

### JWT Tokens

JWT 토큰 방식을 사용하려면 `JwtTokenStore`가 인증 서버에 필요함. 이는 리소스 서버에도 토큰을 디코드하기위해 `JwtTokenStore`가 가지고 있는 `JwtAccessTokenConverter`가 필요함. 토큰은 기본적으로 서명되며 리소스 서버도 또한 이 서명을 검증(verify the signature)할 수 있어야 함. 모든 서버는 인증 서버와 암호화된 토큰을 교환하기 위해 대칭키(symmetric key, shared secret, signing key)가 있거나 또는 대칭키(verifier key-signing key, public-private key, asymmetric key)가 있어야 함. 만약 public key 방식을 사용한다면 인증 서버는 기본적으로 `/oauth/token_key` 엔드포인트로 이를 발급하고 기본적으론 `denyAll()` 설정되어 있음. 사용자는 `SpEL expression`을 사용해서 이를 오픈할 수 있으며 이는 `AuthorizationServerSecurityConfigurer`에서 설정할 수 있음.(e.g. "permitAll()" is probably adequate since it is a public key).

To use the `JwtTokenStore` you need "spring-security-jwt" on your classpath (you can find it in the same github repository as Spring OAuth but with a different release cycle).

### Grant Types

`AuthorizationServerEndpointsConfigurer`에 정의되어 있는 `AuthorizationEndpoint`를 통하여 권한을 부여하는 방법을 지원한다. 기본적으로 모든 권한 부여 방법은 패스워드를 제외하도록 되어있다.

* `authenticationManager`  - `AuthenticationManager`를 인젝팅하여 패스워드 정책을 변경할 수 있음
* `userDetailsService` - if you inject a `UserDetailsService` or if one is configured globally anyway (e.g. in a `GlobalAuthenticationManagerConfigurer`) then a refresh token grant will contain a check on the user details, to ensure that the account is still active
* `authorizationCodeServices` : 인증 코드 부여를 위한 인증 코드 서비스를 정의(`AuthorizationCodeServices`로 구성함). 
* `implicitGrantService`: manages state during the imlpicit grant.
* `tokenGranter`: the  `TokenGranter`  (taking full control of the granting and ignoring the other properties above)

### Configuring the Endpoint URLs

`AuthorizationServerEndpointsConfigurer`은 `pathMapping()` 메소드를 가지고 있음. 이 메소드는 두가지 아규먼트를 가짐.

* The default (framework implementation) URL path for the endpoint
* The custom path required (starting with a "/")

framework에서 제공하는 url은 역할에 따라 몇가지 카테고리로 나뉘어져있다.

* `/oauth/authorize` - 인증을 담당
* `/ouath/token` - 토큰을 담당
* `/oauth/confirm_access` - user posts approval for grants here
* `/oauth/error` - used to render errors in the authorization server
* `/oauth/check_token` - used by Resource Servers to decode access tokens
* `/oauth/token_key` - exposes public key for token verification if using JWT tokens











## refer to
[OAuth 2 Developers  Guide](https://projects.spring.io/spring-security-oauth/docs/oauth2.html)






















<!--stackedit_data:
eyJoaXN0b3J5IjpbMTUxOTI2NDM4NSw3MzI1NTc3OTEsMzMzOD
MzNDIxLC0xOTk3MjE2MzkwLC0xNjM0Njk2MTMsLTE0MTczMTMx
OCwxMTMxMjgwMjUsNzMwOTk4MTE2XX0=
-->