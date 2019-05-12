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

일반적으로 쓰레드의 경우는 각각 별개로 움직여서 쓰레드간에 시작/종료가 다른 쓰레드에 영향을 미치지 않음. 예를들어 메인쓰레드에서 실행시킨 쓰레드1~10은 각각의 쓰레드가 실행되거나 종료되거나 상관 없이 각자 할일을 마치고 사라지는 형태임. 하지만 ```Callable```을 사용하여 리턴값을 받아야 하면 그 결과를 계산하는 쓰레드가 끝날 때 까지 기다려야 함. 이 부분에서 ```Future```가 유용하게 사용되는데 



<!--stackedit_data:
eyJoaXN0b3J5IjpbODk0MDEyNjE5LC0xMDM5MzQxNTA2LC0xOT
U4MDg4NTQyXX0=
-->