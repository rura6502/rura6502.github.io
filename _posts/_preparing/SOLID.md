---  
layout: post  
title:  SOLID
date:  2020-08-16 00:00:00 +0900  
categories: [SOLID, Java, OOP]
---
# SOLID
* S : SRP, Single Responsibility Principle, 단일 책임 원칙
* O : Open Closed Principle
* L : Liskov Substitution Principle
* I : Interface Segregation Principle
* D : Dependency Inversion Principle

## S, SRP
Single Responsibility Principle, 단일 책임 원칙.
* 한 클래스는 한 기능만 책임진다.
* 클래스가 바뀌어야 하는 이유는 오직 하나이다.

```java
class Book {
	private String author;
	private String title;
	private double price;

	
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbOTYwMDc0NDQsMTQwNTI4MzYyNiw4NTA3NT
QzMzNdfQ==
-->