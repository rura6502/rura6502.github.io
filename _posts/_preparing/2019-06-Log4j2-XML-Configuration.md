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




<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0NDc2MjA4NzMsLTE0OTI2MDM1MCwtMj
k3NjA5ODM2LDExMTI1MjkyMTcsMTE3ODA4NzY3NiwxODQ3ODMz
ODgxXX0=
-->