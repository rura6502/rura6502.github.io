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

## Open Closed Principle
개방, 폐쇄 원칙. 확장에는 열려있고 수정에는 닫혀있다. **확장에 열려있다**라는 의미는 어떠한 클래스가 있을 때, 그 클래스가 할 수 있는 행위들이 필요한 경우에 더 추가될 수 있고 **수정에 닫혀있다**라는 의미는 클래스가 제공하는 기능이 확장될 때, 그 클래스와 연관되어있는 다른 클래스들에게 변화를 미치지 않는다 즉, 다른 클래스 수정 없이도 연관된 클래스의 행위를 확장할 수 있다는 의미이다.

어떻게 기능이 추가되었음에도 그 기능을 쓰는 부분의 변화 없이 쓸 수 있을까? 답은 **추상화**에 있다. 서로 연관되어야 하는 클래스가 직접적으로 연관되지 않고 추상화된 객체가 중간 레이어 역할을 한다면, 확장을 해야하는 클래스에서는 서로 쓰기로 약속된 **추상화된 형태**를 유지하면서 확장할 수 있고 기능을 사용하는 클래스 입장에서 또한 **추상화된 형태**로 그 클래스를 사용하기로 약속하였으므로 별도의 변화 없이 기존에 쓰던 형태로 그대로 쓸 수 있는 것이다.

예를들어 메인보드를 상상해보자. 메인보드는 키보드와 통신하는 방법, 마우스와 통신하는 방법,  플래시 메모리와 통신하는 방법등을 개별적으로 모두 가지고 있지 않다. 만약 메인보드가 모든 주변기기들과 통신하는 방법을 개별적으로 가지고 있다면 이 세상에 메인보드와 통신해야 되는 주변기기들이 생길 때 마다 메인보드는 업그레이드를 하거나 추가로 모듈을 장착해야 하는 상황이 발생할 것이다. 만약 코드로 표현한다면 아래와 같이 표현할 수 있다.

```java
class 키보드 {
  public 받은_데이터 키보드_통신(보낼_데이터) { ... }
}
class 마우스 {
  public 받은_데이터 마우스_통신(보낼_데이터) { ... }
}
class 플래시메모리 {
  public 받은_데이터 플래시메모리_통신(보낼_데이터) { ... }
}

class 메인모드 {
  public 받은_데이터 주변기기와_통신(주변기기, 보낼_데이터) {
    if (주변기기 == 키보드) {
      키보드_통신(보낼_데이터)
	} else if (주변기기 == 마우스) {
	  마우스_통신(보낼_데이터)
	} else if (주변기기 == 플래시메모리) {
	  플래시메모리_통신(보낼_데이터)
	}
	......
	
	// 주변기기가 추가될 경우, if 구문이 계속 추가...
	else if (주변기기 == 스피커) {
      스피커_통신(보낼_데이터)
	} else if (주변기기 == 스마트폰) {
	  플래시메모리_통신(보낼_데이터)
	}
  }
}
```


그래서 우리는 **USB**라는 표준 규격(편의상 버전은 생략)을 두고 주변기기들도 **USB**라는 규격에 맞게 기기를 통신할 수 있게 만들고, 메인보드도 **USB**규격에 맞게 통신하는 방법을 만들어 놓으면 어떠한 기기든 **USB**라는 규격만 맞춘다면 메인보드와 통신할 수 있는 것이다.

다시 OCP로 돌아가자면, 이 예제에서 **이 세상에서 개발되어있는, 또 앞으로 개발될 주변기기**는 **확장(앞으로 개발될)이 필요한 클래스**이고 **USB 규격**은 **추상화된 형태**, **메인보드**는 **확장될 가능성이 있는 클래스를 사용할 클래스** 이다.  OCP의 핵심 구문과 비교하자면

* **확장에 열려있다** : 이 세상에 주변기기가 아무리 다양하게 개발되더라도
* **수정에 닫혀있다** : USB 규격만 맞춘다면 메인보드는 그 모든 주변기기를 위한 별도의 기능이나 장치를 제공할 필요가 없다.

라고 비유할 수 있다. 자바에서는 **USB 규격**과 같이 **추상화된 형태**를 `interface` 또는 `abstract class` 형태로 제공하고 있다. `interface`를 사용하여 OCP를 준수하면 아래와 같이 변할 수 있다.

```java
// 추상화된 형태...
interface USB {
	받은_데이터 통신(보낼_데이터)
}

class 키보드 implements USB {
  @Override
  public 받은_데이터 통신(보낼_데이터) { ... }
}
class 마우스 implements USB {
  @Override
  public 받은_데이터 통신(보낼_데이터) { ... }
}
class 플래시메모리 implements USB {
  @Override
  public 받은_데이터 통신(보낼_데이터) { ... }
}

class 메인모드 {
  public 받은_데이터 주변기기와_통신(USB usb, 보낼_데이터) {
    // 주변기기가 추가되더라도 if문이 추가될 필요가 없다.
    usb.통신(보낼_데이터)
  }
}
```




## L, Liskov Substitution Principle




## I, Interface Segregation Principle
## D, Dependency Inversion Principle

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTAyMzU3MzYyNSw0MTgzMTIxMTQsLTM3Nz
cwODMyLC0xODMxMzMxNDI3LDE1MjMyMDk0MDMsMTQwNTI4MzYy
Niw4NTA3NTQzMzNdfQ==
-->