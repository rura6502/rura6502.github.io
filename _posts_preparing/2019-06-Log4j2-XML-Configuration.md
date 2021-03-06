---  
layout: post  
title:  Log4j2 XML Configuration 
date:  2019-05-21 00:00:00 +0900  
categories: [log4j2]
---

Log4j2의 XML 설정 파일은 아래와 같은 구조로 구성되어 있음.

```xml
<Configuration>
  <Properties>
    <Property></property>
  </Properties>
  <Filter></Filter> <!-- 필터는 여러 곳에 위치할 수 있음, 아래 설명 참조 -->
  <Appenders>
    <Filter></Filter>
  </Appenders>
  <Loggers>
    <Logger>
      <Filter></Filter>
    </Logger>
    <Filter></Filter>
  </Loggers>
</Configuration>
```
 
 ### <Loggers />
 
 LoggerConfig는 logger 엘리먼트로 구성이 됨. logger 엘리먼트는 name 속성을 반드시 가지고 있어야 하고 필요에 따라 level/additivity 속성을 선언하면 됨. 여기서 말하는 level은 흔히 로그에서 쓰이는 TRACE/DEBUG/INFO/WARN/ERROR/ALL/OFF 를 의미함. level을 별도로 선언하지 않을 경우 default로 ERROR가 설정되고 additivity는 default값으로 true를 설정하게 됨.

LoggerConfig는 하나 또는 그 이상의 AppenderRef 엘리먼트를 선언해서 특정 Appeder를 참조할 수 있음. 만약 다수의 Appender를 참조한다면 LoggerConfig는 발생하는 로깅 이벤트를 모두 각각의 Appender로 보냄.

여기서 주의해야 할 점은 모든 Logger들은 별도로 이름이 없이 기본으로 선언되어 있는 root Logger를 상속받게 되는데 root Logger는 level이 error이며 additivity 속성과 이름을 가지고 있지 않음.

### <Appenders />

Appender는 플러그인의 이름을 적거나 Appender 엘리먼트를 적어서 설정할 수 있음. Appender는 반드시 name 속성을 가져야 하며 이 값을 기준으로 logger에게 참조되기 떄문에  Appender들 사이에서 유니크해야 됨. 

### <Filters />

필터는 4가지 장소에 위치할 수 있음.

* <Appenders>, <loggers>, <properties> 엘리먼트와 같은 레벨. 여기에 위치할 경우 로깅 이벤트가 LoggerConfig에 전달되기 전에 accept/reject 여부를 결정함.
* <logger> 엘리먼트에서, 특정 loggers에 전달되기 전에 accept/reject 여부를 결정
* <Appender> 엘리먼트에서, 특정 appender에 전달되기 전에 accept/reject 여부를 결정
* <AppenderRef> 엘리먼트에서, logger가 appender에게 이벤트를 전달해야하는지를 결정

<Filters> 엘리먼트를 이용해서 여러가지 필터를 하나로 묶어서 사용할 수 도 있음.

```xml
<filters>
  <MarkerFilter marker="FLOW" onMatch="ACCEPT" onMismatch="NEUTRAL"/>
  <MarkerFilter marker="EXCEPTION" onMatch="ACCEPT" onMismatch="DENY"/>
</filters>
```

### <Properties />

프로퍼티에 선언된 값들은 `${key}`의 값 형태로 다른 요소들 안에서 사용될 수 있음.



<!--stackedit_data:
eyJoaXN0b3J5IjpbMjAxNzY2NjAxMywxNDYwNzUwMTE3LDQ3OT
cyMDM0OSwyNTYxNDc2MzQsLTE0NDc2MjA4NzMsLTE0OTI2MDM1
MCwtMjk3NjA5ODM2LDExMTI1MjkyMTcsMTE3ODA4NzY3NiwxOD
Q3ODMzODgxXX0=
-->