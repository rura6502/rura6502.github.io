---  
layout: post  
title:  Log4j2 Configuration
date:  2019-05-21 00:00:00 +0900  
categories: [log4j2]
---

![log4j_architecture_class_diagram](https://github.com/rura6502/rura6502.github.io/blob/master/_posts/_image/log4j_architecture_class_diagram.jpg?raw=true)

Log4j의 구조. Application은 Logger를 통해서 Log4j에 접근한다. Logger는 고유의 이름을 가지고 LogManager에 의해서 생성된다. LogManager는 LoggerContext에 있는데 이 LoggerContext는 Configuration과 함께 전체 로그 시스템의 core를 이룬다. loggerConfig는 환경설정 내부에 선언된 Logger 선언에 의해서 생성된다. LoggerConfig와 연결되어있는 Appender는 LogEvents를 어떻게 다룰 것인지(저장/출력)에 대하여 정의한다.

Logger는 항상 LoggerConfig를 가지고 있는데 Logger가 가지고 있는 LoggerConfig는 Logger와 이름이 같거나, 부모 패키지의 이름을 가지거나, Root LoggerConfig이다. 

Log4j2의 환경설정을 위해 이해야 하는 개념들

## Glossary

### LoggerContext

Log4j2의 로그시스템의 중심.

### Configuration

모든 LoggerContext는 active Configuration을 기반으로 동작함. Configuration은 Appenders, context-wide Filters, LoggerConfigs, StrSubstitutor 등 로그 동작에 필요한 모든 값들을 가짐.

### Logger

이름을 가지고 LoggerConfig와 실제 연관되어 실제 로그 작업을 수행하는 주체.

### LoggerConfig

로깅 환경설정 내부에 선언되어 있는 Loggers가 만들어지면 생성되는 객체.  Appenders를 참조하고 있음.

### Log Levels

L:oggerConfig는 Log Level을 가짐. 기본적으로 내정되어 있는 레벨은 TRACE, DEBUG, INFO, WARN, ERROR, FATAL이 있지만 커스텀도 가능함. 앞서 나열한 순서대로 필터는 지정된 로그 레벨 보다 앞에 있는 레벨들을 필터함. 예를들어서 필터의 로그레벨 값이 INFO 이면 DEBUG, TRACE는 표시하지 않음.

### Filter

로그를 자동으로 필터링 해줌. 필터는 아래의 위치에서 동작할 수 있음.

* control이 LoggerConfig로 전달되기 전
* control이 LoggerConfig로 전달되었지만 어떠한 Appenders도 호출되기 전
* control이 LoggerConfig로 전달되었지만  특정 Appender가 호출되기 전
* 개별 Appender에서

방화벽의 필터와 유사하게 동작함. 각각의 필터는 세가지 중 하나의 상태를 반환하는데 Accept, Deny, Neutral이 있음. 

* Accept : 필터가 더이상 호출될 필요 없으며 이벤트가 진행되어야 함.
* Deny : 이벤트가 바로 무시되어야 하며 caller에게 control이 return 되어야 함.
* Neutral : 이벤트가 다른 필터로 전달되어야 함. 만약 다른 필터가 없을 경우 처리됨.

### Appender

logging requests에 대한 enable/disable을 결정하는 것은 매우 일부분. Log4j는 로깅 요청들을 출력할 수 있는 다중 창구(Multiple Destination)을 제원하는데 하나의 아웃풋 창구를 Appender라고 부름. 다양

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwMjQ3NzE1MjYsLTQ0NDkwMjQ3OCwxMj
gwMzY2NjkwLDg0NjgxOTU3NSwtMTQwMDIwMTQ2MywyMDM4Mzc2
NTExXX0=
-->