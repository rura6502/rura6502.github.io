
---  
layout: post  
title:  OAuth 2.0 Authorization Framework 표준
date:  2019-05-21 00:00:00 +0900  
categories: [OAuth2, Standard, IETF]
---

본 문서는 [The OAuth 2.0 Authroization Framework](https://tools.ietf.org/html/rfc6749)를 정리한 문서입니다.

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
B. 클라이언트는 리소스 오너에게 인증정보를 받습니다. 여기서 인증 정보란 지금의 리소스 오너가 진짜 리소스 오너인지를 검증하는 인증정보입니다. 이 인증정보의 형태(grant type)는 OAuth 2.0에서는 기본적으로 4가지 방식과 추가 extension을 정의하고 있습니다. 어떤 인증정보 형태를 사용하는지는 클라이언트가 어떤 방식을 사용할 지, 서버가 어떤 방식을 지원하는지에 따라 갈립니다.
C. 클라이언트는 인증 서버에 엑세스 토큰을 요청합니다.
D. 인증 서버는 클라이언트를 검증하고 '인증 허가'가 유효한지 체크한 다음 유효하다면 엑세스 토큰을 발급합니다.
E. 클라이언트는 인증 서버에서 발급받은 토큰을 사용해 접근이 보호된 리소스를 리소스 서버에 요청합니다. 
F. 리소스 서버는 엑세스 토큰을 검증하고 유효하다면 요청을 처리합니다.

위의 방식의 예로 RFC 문서에서는 사진을 인화해주는 서비스를 예를들고 있습니다.
나는 특정 사진을 인화하고싶은데 이 사진은 내 클라우드 스토리지에 저장되어 있는 상황입니다.
1. 나(리소스오너)는 인화 서비스(클라이언트)에게 클라우드 스토리지(리소스 서버)에 접근할 수 있는 인증정보를 줍니다. 이때 인증정보란 클라우드 스토리지 서비스에 접근할 수 있는 나의 아이디/비밀번호 입니다.
2. 인화 서비스는 나의 인증정보를 가지고 리소스



## refer to
[The OAuth 2.0 Authorization Framework](https://tools.ietf.org/html/rfc6749)


























































<!--stackedit_data:
eyJoaXN0b3J5IjpbMTU3OTE1MTA5NSwtNjYzMzM0MDg0LC0xMD
k1ODIxOTUxLDEwMjcxMzIzNDhdfQ==
-->