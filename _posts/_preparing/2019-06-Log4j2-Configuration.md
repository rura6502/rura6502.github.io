---  
layout: post  
title:  Log4j2 Configuration
date:  2019-05-21 00:00:00 +0900  
categories: [log4j2]
---

Log4j2의 환경설정을 위해 이해야 하는 개념들

### LoggerContext

Log4j2의 로그시스템의 중심.

### Configuration

모든 LoggerContext는 active Configuration을 기반으로 동작함. Configuration은 Appenders, context-wide Filters, LoggerConfigs, StrSubstitutor 등 로그 동작에 필요한 모든 값들을 가짐.


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3MTk1NjU5MTJdfQ==
-->