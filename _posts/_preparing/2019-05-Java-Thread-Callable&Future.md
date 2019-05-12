---
layout: post
title:  Java Thread Callable & Future
date:  2019-05-11 00:00:00 +0900
categories: [Thread, Java, Spring, Spring Boot]
---

## Callable & Future

자바의 쓰레드는 별도의 반환값(return)이 없음.

```java
public interface Runnable {
    public abstract void run();  // 반환이 void 이다.
}
```
이러한 반환값을 받을 수 없는 단점을 보완하는 ```java.util.concurrent.Callable``` 인터페이스가 있는 이를 사용하여 리턴값을 받을 수 있음. ```Callable```을 ```ExecutorService.sutmit``` 메소드로 실행시켜서 쓰레드로 실행시킬 수 있는데 여기서 리턴값은 ```java.util.concurrent.Future``` 로 반환함. 

일반적으로 쓰레드의 경우는 각각 별개로 움직여서 쓰레드간에 시작/종료가 다른 쓰레드에 영향을 미치지 않음. 예를들어 메인쓰레드에서 실행시킨 쓰레드1~10은 각각의 쓰레드가 실행되거나 종료되거나 상관 없이 각자 할일을 마치고 사라지는 형태임. 

> 예시. 직원1이 직원2에게 할일2를 주고 자기 자신은 할일1을 한다. 직원1은 할일2가 끝나면 자기한테 그 결과를 말하지 않아도 되며 바로 퇴근하라고 한다.

하지만 앞서 기술한데로 ```Callable```은 반환값을 가지며 이 반환값을 받기 위해선 당연하게도 ```Callable```로 실행되는 쓰레드가 끝날 때 까지 기다려야 함.

> 예시. 직원 

을 사용하여 리턴값을 받아서 사용해야 하는 경우엔 리턴값을 반환하는 쓰레드가 끝날 때 까지 기다려야 하고 기다리는 동안 아무것도 못하는 상황이 발생함. 사실상 쓰레드가 제공하는 비동기성을 기대할 수 없음.. 이 부분에서 ```Future```가 이 기다림을 일정부분 유용하게 사용되는데 



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2MzY1ODQ5NjYsLTEwMzkzNDE1MDYsLT
E5NTgwODg1NDJdfQ==
-->