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
단일책임 원칙에 의해 `책이라는 도메인을 모델링하는 객체`와 `책이라는 도메인을 저장하는 객체`로 나눈다.
```java
class Book {
	private String author;
	private String title;
	private double price;
}
```
```java
class BookService {
	public Book save(Book book) {
		// DB Connect
		// execute query to add Book row
		// Close DB	
	}
}
```
`BookService` 클래스도 `Book 클래스에 대한 비즈니스 로직`과 `DB 컨트롤(접속, 해제)`부분으로 `책임`을 나눌 수 있다.

```java
class DBManager {
	public DBConnection getConnection() { /*..*/ }
	public DBConnection closeConnection() { /*..*/ }
}
```
```java
class BookService {

	private DBManager dbManager;
	public Book save(Book book) {
		DBConnection conn = dbManager.getConnection();
		// save logic
		dbManager.closeConnection();
	}
}
```

위와같이 `데이터베이스에 대한 책임`을 분리할 경우 다른 클래스에서도 DB접속 로직이 필요할 때 `DBManager` 클래스는 데이터베이스에 대한 `단일책임`만을 가지고 있으므로 다른 비즈니스로직에 영향을 끼치지 않도록 안전하게 가져다 쓸 수 있다.(커넥션등의 문제는 별도). 이렇게하면 개별 클래스가 하나의 책임만을 취급하게 되므로 한 클래스가 여러 책임을 가지고 사이즈가 거대해지는 `God Class`를 방지(`God Class Anti Pattern`)할 수 있다. 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4MzEzMzE0MjcsMTUyMzIwOTQwMywxND
A1MjgzNjI2LDg1MDc1NDMzM119
-->