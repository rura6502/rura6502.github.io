---  
layout: post  
author: rura6502
title:  프로그램 설계 원칙 - KISS, DRY
date:  2020-08-16 00:00:00 +0000  
categories: [Language, Java]
tags: [OOP,Joop, java]
math: true
---
# 프로그램 설계 원칙 - KISS, DRY, Decoupling
## KISS, Keep it short and simple
코드는 항상 명확하고 깔끔하게 작성해라. 복잡하고 간결하지 않은 코드는 상대방을 이해시키기 어렵고 내일의 나조차도 보기 힘들다.
* Bad
	 ```java
	 public String getDayString(int sequence) {
	   if (sequence == 0) {
		   return "Sunday";
	   } else if (sequence == 1) {
		   return "Monday";
	   } else if (sequence == 2) {
		   return "Tuesday";
	   } else if (sequence == 3) {
		   return "Wednesday";
	   } else if (sequence == 4) {
		   return "Thursday";
	   } else if (sequence == 5) {
		   return "Friday";
	   } else if (sequence == 6) {
		   return "Saturday";
	   } else {
		   return "unknown";
	   }
	 } 
   ```
 * Good
    ```java
    public String getDAyString(int sequence) {
	  if (sequence > 6)
		  return "unknown";
	  
	  string[] days = {
		  "Monday", "Tuesday", "Wednesday"
		  , "Thursday", "Friday", "Saturday", "Sunday"
	  }
	  return days[sequence];
    }
   ```
   
## DRY, Don't Repeat Yourself
코드의 중복을 없앤다. 중복된 코드가 많을 경우 해당하는 코드에 이상이 생겼을 때 같은 코드가 적용된 모든 부분을 변경해야 하는 번거로움이 생기며 이는 변경해야 하는데 깜빡하고 변경하지 못해서 버그가 생길 경우의 수를 남겨두게 된다.

* Bad
	```java
	public List<User> getUsers() {
		// load driver
		// DB connect
		// execute qeury for getting user
		// DB close
	}
	public User addUser(User user) {
		// load driver
		// DB connect
		// execute qeury for adding user
		// DB close
	}
	```
* Good
	```java
	public void readyToUseDB() {
		// load dirver
		// DB Connect
	}
	public void closeDB() {
		// DB Close
	}
	public List<User> getUsers() {
		readyToUseDB()
		// execute qeury for getting user
		closeDB()
	}
	public User addUser(User user) {
		readyToUseDB()
		// execute qeury for adding user
		closeDB()
	}	
	```

## Decoupling
결합도(coupling) 높다는 뜻은 어떤 클래스가 동작하기 위해서 특정 클래스와 그 구현에 의존성(얼마나 깊이 참조했는가)이 높다는 것을 의미한다. 결합도가 높을 경우 특정 클래스의 변화가 다른 클래스에 영향을 많이 미칠 수 있어 변화에 취약하다는 것을 의미한다.

이러한 커플링을 중간의 Layer역할을 해줄 `interface`를 활용해서 결합도를 줄일 수 있다. AA건전지를 사용하는 리모컨 을 생각해보자. 모든 브랜드의 건전지가 동일한 스펙(파워, 지속시간 등의 성능, 클래스에서는 input, output에 해당함)이라고 가정했을 때, 에너자이저가 내부적으로(클래스/메소드의 구현) 어떻게 전기를 발생시키는지, 듀라셀과는 어떤 차이인지에 대해 알필요가 없다. 표준인 AA규격(가로 xx cm, 세로 xx cm, 양쪽 끝의 +, -극, 출력 등)만 맞춘다면 그냥 끼우면 리모컨을 사용할 수 있다.

![enter image description here](https://github.com/rura6502/rura6502.github.io/blob/master/_posts/_image/bettry_interface.png?raw=true)

클래스도 동일하다. 규격만 정해져 있고 각 클래스는 해당 규격을 사용해서 그 클래스가 맡은 일에 대한 `책임`만 수행해낸다면, 클래스A가 클래스B를 사용할 때 굳이 클래스B가 내부적으로 어떻게 동작하는지 클래스A가 신경쓸 필요가 없고 클래스B가 내부적으로 변한다고 해도 규격은 약속되어 있으니 변화 없이 사용할 수 있다. 또한 서로 약속한 규격만 사용하고 그 책임만 수행해낸다면 클래스A는 클래스C를 써도되고 D를 써도된다. 

![enter image description here](https://github.com/rura6502/rura6502.github.io/blob/master/_posts/_image/decoupling_using_interface.png?raw=true)

내 할일 목록에 대한 CRUD API를 제공한다고 했을 때 CSV 파일 베이스로 정보를 제공할 경우 아래와 같이 구성할 수 있다.


```java
public class TodoCSVReader {
  public List<Todo> readAllTodoFromCSV(...) { ... }
}
```
```java
public class TodoAPI {
  TodoCSVReader todoCSVReader;

  public List<Todo> getTodos() { 
	  return todoCSVReader.readAllTodoFromCSV(...);
  }
}
```
이는 `TodoAPI`클래스가 `TodoCSVReader`클래스와 직접 연결되어, 만약 Todo데이터를 제공하는 리소스가 CSV에서 데이터베이스 또는 엑셀의 xlsx로 변경될 때, 단지 Todo 데이터 제공 API 역할을 하는 `TodoAPI` 클래스도 변경되어야 함을 의미한다. 정리하자면 `TodoAPI` 클래스는 외부로 Todo 데이터를 제공하는 역할만을 책임지고 담당했지만, 리소스로부터 Todo을 읽어오는 역할에 대한 책임을 지는 클래스가 변경이 되면 그 영향을 받는 것이다.

이러한 결합, 즉 커플링을 해소하기 위해 java에서 제공하는 `interface`를 사용해서 중간 레이어를 추가하게되면 아래와 같이 구성할 수 있다.

```java
public interface TodoReader {
  public List<Todo> readAllTodoFromResource(...);
}
```
```java
public class TodoAPI {
  TodoReader todoReader;
  public List<Todo> getTodos() {
    return todoReader.readAllTodoFromResource(...);
  }
}
```

중간 인터페이스 레이어인 `TodoReader` 인터페이스를 사용하고 필요에 따라 `TodoAPI`에 `TodoReader`에 대한 구현체를 실행 시 주입하면 `TodoAPI`는 `TodoReader`의 책임으로부터 자유로워져 결합도를 낮출 수(decoupled) 있다.


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2NTM5Mjc4MzUsLTE4OTU1NjIzODksLT
EwNDY3NDg0NjgsLTE1NDc1MjE4ODQsMjAwNDQyODQ3Nyw4MDIz
NDI0MDIsLTE5MDgzODM1ODIsOTAzMzcyODg0LC04MDk5MDYzNj
ZdfQ==
-->