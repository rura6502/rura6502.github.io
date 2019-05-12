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
이러한 반환값을 받을 수 없는 단점을 보완하는 ```java.util.concurrent.Callable``` 인터페이스가 있어 이를 사용하여 리턴값을 받을 수 있음. 

일반적으로 쓰레드의 경우는 각각 별개로 움직여서 쓰레드간에 시작/종료가 다른 쓰레드에 영향을 미치지 않음. 예를들어 메인쓰레드에서 실행시킨 쓰레드1~10은 각각의 쓰레드가 실행되거나 종료되거나 상관 없이 각자 할일을 마치고 사라지는 형태임. 

> 직원1이 직원2에게 할일2를 맡기고 자기 자신은 할일1을 한다. 직원1은 할일2가 끝나면 자기한테 그 결과를 말하지 않아도 되며 바로 퇴근하라고 한다.

하지만 앞서 기술한데로 ```Callable```은 반환값을 가지며 이 반환값을 받기 위해선 당연하게도 ```Callable```로 실행되는 쓰레드가 끝날 때 까지 기다려야 함.

> 직원1이 직원2에게 할일2의 결과물을 받으려면 직원2가 할일2를 모두 끝낼 때 까지 기다려야 한다.

```Callable```을 실행시킬 수 있는 ```ExecutorService.submit```은 ```Future```을 반환하는데 이는 일종의 티켓이나 확인찬스? 개념으로 사용됨. ```Callable```로 실행되는 쓰레드를 기다리지 않고 자기 할일을 하면서 나중에 그 리턴값이 필요할 경우에 리턴값을 ```Future.get()```으로 호출하여 반환받을 수 있음.

> 직원1이 직원2에게 할일2을 맡기면 직원2는 직원1에게 '내가 할일2를 진행하고 있을께요. 당신은 할일1을 하고있다가 할일2에 대한 결과물이 필요해지면 나한테 말하세요' 라고 말한다.

```java
public class FutureBasic {
  public static void main(String[] args) throws InterruptedException, ExecutionException {  // 직원1
    System.out.println("[main] start @" + Thread.currentThread().getName());
    ExecutorService es = Executors.newSingleThreadExecutor();
    System.out.println("[main] start thread ---");
    Future<String> result = es.submit(new CallableTest());
    System.out.println("[main] this is main job ---");
    System.out.println("[main] I will wait Futre...");
    System.out.println(result.get());
  }
}
class CallableTest implements Callable<String> {  // 직원2
  @Override
  public String call() throws Exception {
    System.out.println("[Thread] Job start in call() is @" + Thread.currentThread().getName());
    Thread.sleep(1000);
    System.out.println("[Thread] Job done in call()");
    return "Hi!";
  }
  
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1MzcyOTkwNjcsLTEwMzkzNDE1MDYsLT
E5NTgwODg1NDJdfQ==
-->