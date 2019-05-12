---
layout: post
title:  Java Executor, ExecutorService
date:   2019-05-11 00:00:00 +0900
categories: [Java, Thread, Executor, ExecutorService]
---

## Java Executor, ExecutorService

JDK에서는 Executor 인터페이스를 사용하여 태스크를 관리 함. 태스크란 쓰레드가 특정 목적을 가지고 어떠한 작업을 수행할 때 태스크(Task) 라는 개념으로 부름.(쓰레드와 거의 동일한 개념으로 많이 불리어 지는 듯 함) ExecutorService는 테스크의 풀(Pool)을 만들어 관리하는 역할을 함.

ExecutorService를 생성하는 방식은 다양한데 대표적으로 static으로 선언되어져 있는 Executors를 사용하여 만들 수 있음

```java
ExecutorService executorService = Executors.new******* // 다양한 ExecutorService, Pool을 만들 수 있음
```

쓰레드 운용 특성에 따라 여러가지 쓰레드 풀을 생성할 수 있음

* newSingleSthreadExecutor : 쓰레드를 하나만 운용. 하나만 운용하므로 Thread-Safe를 보장함.
* 


#### refer to
[java-executor-service-tutorial](https://www.baeldung.com/java-executor-service-tutorial)
[Executors (Java Platform SE 7 )](https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/Executors.html)
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwNjM2NTAxMjEsLTE5MjM0NjcxNjUsLT
EwNjE0MTE1MjcsMjExMzM5MDE0NSwtODg5NzIxNjgwXX0=
-->