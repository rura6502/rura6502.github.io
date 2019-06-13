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

### <Configuration />

Configuration 태그에서 사용할 수 있는 대표적인 기능들

* Automitic Reconfiguration : 로그의 설정 파일(log4j2*.*) 내용이 변경되면 별도의 재시작이나 재적용 작업 없이 그 설정을 적용시키는 기능
* Advertise : 잘 모르겠음
* 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI4NzAwMjA1NSwxMTEyNTI5MjE3LDExNz
gwODc2NzYsMTg0NzgzMzg4MV19
-->