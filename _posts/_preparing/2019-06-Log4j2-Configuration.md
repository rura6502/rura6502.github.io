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

### Logger

이름을 가지고 LoggerConfig와 실제 연관되어 실제 로그 작업을 수행하는 주체.

### LoggerConfig

로깅 환경설정 내부에 선언되어 있는 Loggers가 만들어지면 생성되는 객체.  Appenders를 ㅊㅁ
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjA3Nzk4MzAxMV19
-->