---
layout: post
title:  Java Thread Callable & Future
date:  2019-05-11 00:00:00 +0900
categories: [Thread, Java, Spring, Spring Boot]
---

## Callable

자바의 쓰레드는 별도의 반환값(return)이 없음.

```java
public interface Runnable {
    public abstract void run();  // 반환이 void 이다.
}
```
```java.util.concurrent.Callable```을 사용하여 리턴값을 받을 수 있음. ```Callable```을 ```ExecutorService.sutmit(```로 실행시켜서 하지만 리턴값은 하지만 일반 쓰레드의 경우는 각각 별개로 움직여서 쓰레드간에 시작/종료가 다른 쓰레드에 영향을 미치지 않음. 

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyNTg4MzAxMTksLTEwMzkzNDE1MDYsLT
E5NTgwODg1NDJdfQ==
-->