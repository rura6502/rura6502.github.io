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

	public Book save(Book book) {
		// DB Connect
		// execute query to add Book row
		// Close DB
	}
}
```
위 클래스는 책이라는 현실 도메인을 `Book`이라는 객체로 모델링한 클래스인데 `Book`과는 연관없는 아래의 이유로 변경되어야할 수 있다.
* 데이터 베이스 접속 방법이 변경되거나 DB가 변경되어서 다른 모양의 쿼리를 실행해야 함.
* DB로 저장하는 것이 아니라 파일 등의 다른 타입으로 저장함
단일책임 원칙에 의해 책이라는 도메인을 모델링하는 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2NzY5MTQzLDE0MDUyODM2MjYsODUwNz
U0MzMzXX0=
-->