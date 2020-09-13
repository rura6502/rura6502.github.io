---
layout: post
autor: rura6502
title:  Java Executor, ExecutorService
date:   2019-05-12 00:00:00 +0000
categories: [Language,Java]
tags: [Executor,Java,Thread]
math: true
---

## Java Executor, ExecutorService

JDK에서는 Executor 인터페이스를 사용하여 태스크를 관리 함. 태스크란 쓰레드가 특정 목적을 가지고 어떠한 작업을 수행할 때 태스크(Task) 라는 개념으로 부름.(쓰레드와 거의 동일한 개념으로 많이 불리어 지는 듯 함) ExecutorService는 테스크의 풀(Pool)을 만들어 관리하는 역할을 함.

ExecutorService를 생성하는 방식은 다양한데 대표적으로 static으로 선언되어져 있는 Executors를 사용하여 만들 수 있음

```java
ExecutorService executorService = Executors.new******* // 다양한 ExecutorService, Pool을 만들 수 있음
```

쓰레드 운용 특성에 따라 여러가지 쓰레드 풀을 생성할 수 있음

* newFixedThreadPool : 정해진 크기(쓰레드 갯수)를 가진 풀을 생성함. 가용할 수 있는 모든 쓰레드를 모두 사용 중일 경우에 여유 쓰레드가 생길 때 까지 큐에 대기시킴. 
```java
ExecutorService es = Executors.newFixedThreadPool(5);
for (int i=0; i<10; i++) {
  es.execute(() -> {
    System.out.println(Thread.currentThread().getName());
  });
}
// 결과 : 5개의 스레드가 활동하였음.
pool-1-thread-1
pool-1-thread-2
pool-1-thread-2
pool-1-thread-3
pool-1-thread-3
pool-1-thread-3
pool-1-thread-4
pool-1-thread-2
pool-1-thread-1
pool-1-thread-5
```
* newSingleThreadExecutor: 쓰레드를 하나만 운용. 하나만 운용하므로 Thread-Safe(동기화 문제가 발생하지 않음)를 보장함. ```newFixedThreadPool(1)```과 똑같이 동작함.
```java
ExecutorService es = Executors.newSingleThreadExecutor();
for (int i=0; i<3; i++) {
  es.execute(() -> {
	System.out.println(Thread.currentThread().getName());
  });
}
// 결과 : 1개의 쓰레드만 활동하였음.
pool-1-thread-1
pool-1-thread-1
pool-1-thread-1
```
* newCachedThreadPool : 쓰레드를 필요한 갯수만큼 생성하는데 이전에 생성한 쓰레드를 재활용 함. 많은 short-lived asynchronous task를 생성하여 성능을 향상시킬 수 있음. 60초 동안 사용되지 않은 쓰레드는 풀에서 삭제됨. 그러므로 충분한 시간동안 사용되지 않은 풀 그 자체는 어떠한 쓰레드 리소스도 소모하지 않음.  ```ThreadPoolExecutor```를 사용하여 생성할 경우 세부 프로퍼티를 설정할 수 있음(thread timeout 등)
* newSingleThreadScheduledExecutor : ```newSingleThreadExecutor```와 같으나 스케쥴링, 딜레이, 주기 등을 설정할 수 있음.
* newScheduledThreadPool : 스케쥴링, 딜레이, 주기 등을 설정할 수 있는 쓰레드 풀을 생성.
* newWorkStealingPool : jdk1.8 부터 추가, 파라미터로 전달된 '병렬 처리 수준(Parallelism Level)'에 맞는 적절한 스레드 갯수를 생성하여 운용. 또한 경합(Contention)을 줄이기 위한 다중 큐(Multiple Queue)를 운용. 여기서 '병렬 처리 수준' 이란 테스크를 처리하기 위해 사용되어지는 중이거나 사용할 수 있는 스레드의 최대 갯수임. 실제 풀에 있는 쓰레드 갯수는 유동적으로 움직임. 실행 순서를 보장하지 않음.

## refer to
[java-executor-service-tutorial](https://www.baeldung.com/java-executor-service-tutorial)
[Executors (Java Platform SE 7 )](https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/Executors.html)
[Java Single Thread Executor](https://farenda.com/java/java-single-thread-executor/)
 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExNTEyNzU2NTksOTU5MjI5Nzg5XX0=
-->