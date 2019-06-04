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

* `OAuth2AuthenticationProcessingFilter` - 리퀘스트와 함께 온 인증 엑세스 토큰의 정보를 로드하는데 사용



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExMTU1ODM1NCwxNDczNTExNTE0LC0yMD
ExNzEyMTc5XX0=
-->