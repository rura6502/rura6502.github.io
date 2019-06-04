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

프로바이더 설정에서의 중요한 점은 OAuth client에게 인증 코드를 제공하는 방법을 정의하는 것이다. 인증 코드는 OAuth client가 end-user에게 '인증 서버에서 제공하는 인증 페이지에서 인증을 받아서 

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTM3ODE5NDExLC0yNjk0Nzk1NzEsMTQ3Mz
UxMTUxNCwtMjAxMTcxMjE3OV19
-->