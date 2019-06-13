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
 
 LoggerConfig는 logger 엘리먼트로 구성이 됨. logger 엘리먼트는 name 속성을 반드시 가지고 있어야 하고 필요에 따라 level/additivity 속성을 선언하면 됨. 여기서 말하는 level은 흔히 로그에서 쓰이는 TRACE/DEBUG/INFO/WARN/ERROR/ALL/OFF 를 의미함. level을 별도로 선언하지 않을 경우 default로 ERROR가 설정되고 additivity는 default값으로 true를 설정하게 됨.




<!--stackedit_data:
eyJoaXN0b3J5IjpbMTU4MTAwMTY5MywtMjk3NjA5ODM2LDExMT
I1MjkyMTcsMTE3ODA4NzY3NiwxODQ3ODMzODgxXX0=
-->