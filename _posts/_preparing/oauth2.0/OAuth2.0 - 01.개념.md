
---  
layout: post  
title:  OAuth 2.0 Authorization Framework 표준
date:  2019-05-21 00:00:00 +0900  
categories: [OAuth2, Standard, IETF]
---

본 문서는 [The OAuth 2.0 Authroization Framework](https://tools.ietf.org/html/rfc6749)를 정리한 문서입니다. 본 문서에서 사용하는 한글 용어는 제가 이해한 

## Introduction

OAuth 2.0(이하 OAuth)은 HTTP 프로토콜 베이스로 설계된 인증 프레임워크입니다. Third-party application이 사용자의 인증정보(아이디, 비밀번호 등)를 저장함으로써 생기는 다양한 문제들을 토큰을 사용함으로써 해결하고자 하는 것이 목적입니다. 

OAuth의 인증 플로우에는 몇가지 주체들이 등장합니다.

* Resource owner : 보호되어 있는 리소스(protected resource)들에 접근이 가능한 리소스의 주인입니다.
* Resource server : 보호되어 있는 리소스트를 보관하고 서비스하는 서버입니다.
* Client : 리소스 오너를 대신해서 보호되어 있는 리소스에 접근하기 위한 요청을 만드는 애플리케이션입니다. 여기서 클라이언트란 애플리케이션을 실행하는 서버, 데크스탑, 장비 등 다양한 것?일 수도 있습니다.
* Authorization server : 리소스 오너의 인증정보가 유효한지에 대하여 판단하고 엑세스토큰을 발급하는 서버입니다.

### Basic Flow

OAuth의 기본적인 인증 플로우는 다음과 같습니다.

![https://github.com/rura6502/rura6502.github.io/blob/master/_posts/_image/oauth2.0_abstract_protocol_flow.jpg?raw=true](https://github.com/rura6502/rura6502.github.io/blob/master/_posts/_image/oauth2.0_abstract_protocol_flow.jpg?raw=true)

A. 클라이언트는 리소스 오너에게 인증을 요청합니다. 이 인증요청은 리소스 오너가 바로 인증 서버에 요청하는 경우도 있지만 인증 서버를 통해 받도록 하는 중개인의 역할을 수행합니다.
B. 클라이언트는 '인증 허가(Authorization grant)'를 받습니다. 인증 허가란 인증에 대한 허가가 아니라 인증서버에게 요청할 수 있는 



## refer to
[The OAuth 2.0 Authorization Framework](https://tools.ietf.org/html/rfc6749)


























































<!--stackedit_data:
eyJoaXN0b3J5IjpbLTg2OTI5NDM1MSwtNjYzMzM0MDg0LC0xMD
k1ODIxOTUxLDEwMjcxMzIzNDhdfQ==
-->