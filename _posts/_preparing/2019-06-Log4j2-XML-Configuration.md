---  
layout: post  
title:  Log4j2 XML Configuration 
date:  2019-05-21 00:00:00 +0900  
categories: [log4j2]
---

Log4j2의 XML 설정 파일은 아래와 같은 구조로 구성되어 있음.

```xml
<Configuration>
	<Appenders>
		<!-- define Appender --!>
	</Appenders>
	<Loggers>
		<!-- define Logger --!>
	</Loggers>
</Configuration>
```
 
 ### <Loggers />
 
 LoggerConfig는 logger 엘리먼트로 구성이 됨. logger 엘리먼트는 반드시 name, level 속성을 가지고 있어야 하고 필요에 따라 additivity를 가질


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5NDY1NTMzMiwtMjk3NjA5ODM2LDExMT
I1MjkyMTcsMTE3ODA4NzY3NiwxODQ3ODMzODgxXX0=
-->