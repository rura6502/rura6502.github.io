---
layout: post
title:  Spring JPA Speficiation
date:  2019-05-11 00:00:00 +0900
categories: [JPA, Specifications, Spring, Spring-Boot]
---
얼마전 Spring Boot, JPA 기반에 Method Query와 JQPL(복잡하거나 메소드 명이 지나치게 길어질 경우 사용)을 혼용하여 사용하고 있었다.

사용하고 있는 도중 2, 3개정도의 WHERE Condition 까진 괜찮았으나 그 이상 조건이 넘어가거나 3개 정도만 되더라도 입력이 있을 경우 넣고 없을 경우 빼고 하는 등을 고려할 때 메소드 이름이 너무 길어짐, if문을 많이 넣어야됨, 복잡함 등의 문제들이 발생했다.

```java
// 컬럼 A, B, C가 있을 경우

if (A == null) // B or C
	return repository.findAllByBOrC(...)
else if (B == null) // A or C
	return repository.findAllByAOrC(...)
else if (C == null)
	return (repository.findAllByAOrC
else if .....
```

그래서 이런방법을 해결할 수 있는 방법이 없을까 해서 찾아봤는데 ```Specification```이라는 나만 모르고 있었던 것이 있었다.





## refer to
[Use Criteria Queries in a Spring Data Application](https://www.baeldung.com/spring-data-criteria-queries)
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTAwNzkzNDg3MiwxNjAyMTYyNzI2LC0xMD
IwNTQ2NjkyLC0xNzE2MDQzNzUzLC0xNDI4NjY4NzQ3XX0=
-->