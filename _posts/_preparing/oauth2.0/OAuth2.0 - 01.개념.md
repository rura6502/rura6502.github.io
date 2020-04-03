
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


## refer to
[The OAuth 2.0 Authorization Framework](https://tools.ietf.org/html/rfc6749)


























































<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwOTU4MjE5NTEsMTAyNzEzMjM0OF19
-->