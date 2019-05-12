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

> 직원1이 직원2에게 작업2를 맡기고 자기 자신은 작업1을 한다. 직원1은 작업ㅈ2가 끝나면 자기한테 그 결과를 말하지 않아도 되며 바로 퇴근하라고 한다.

하지만 앞서 기술한데로 ```Callable```은 반환값을 가지며 이 반환값을 받기 위해선 당연하게도 ```Callable```로 실행되는 쓰레드가 끝날 때 까지 기다려야 함.

> 직원1이 직원2에게 작업2의 결과물을 받으려면 직원2가 작업2를 모두 끝낼 때 까지 기다려야 한다.

```Callable```을 실행시킬 수 있는 ```ExecutorService.submit```은 ```Future```을 반환하는데 이는 일종의 티켓이나 확인찬스? 개념으로 사용됨. ```Callable```로 실행되는 쓰레드를 기다리지 않고 자기 작업을 하면서 나중에 그 리턴값이 필요할 경우에 리턴값을 ```Future.get()```으로 호출하여 반환받을 수 있음.

> 직원1이 직원2에게 작업2을 맡기면 직원2는 직원1에게 '내가 작업2를 진행하고 있을께요. 당신은 작업1을 하고있다가 작업2에 대한 결과물이 필요해지면 나한테 말하세요' 라고 말한다.

```java
public class FutureBasic {
  public static void main(String[] args) throws InterruptedException, ExecutionException {  // 직원1
    System.out.println("[main] start @" + Thread.currentThread().getName());
    ExecutorService es = Executors.newSingleThreadExecutor();
    System.out.println("[main] start thread ---");
    Future<String> result = es.submit(new CallableTest());  // 직원2에게 작업2를 요청하고 나중에 확인할 수 있는 찬스?를 얻는다.(Future)
    System.out.println("[main] this is main job ---");  // 직원2에게 작업2를 줬으니 직원1은 작업1을 한다.
    System.out.println("[main] I will wait Futre...");
    System.out.println(result.get());  // 작업2에 대한 결과물이 필요하니 직원2에게 작업2에 대한 결과물을 달라고 한다(get() 호출)
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

// 결과 : 직원1(main)은 직원2(Thread)에게 작업2를 주고 자기는 작업1을 수행하였다. 나중에 작업2에 대한 결과물이 필요해지자 직원2에게 물어서(get() 호출) 그 결과물을 받았다.
[main] start @main
[main] start thread ---
[main] this is main job ---
[main] I will wait Futre...
[Thread] Job start in call() is @pool-1-thread-1
[Thread] Job done in call()
Hi!
```

여기서 유의해야 할 점은 어쨋든 ```Future```의 결과물을 받기 위해선 최종적으로 해당 Future를 발행한 쓰레드가 반환할 때 까지 기다려야 한다는 것이다. ```Future``` 인터페이스는 다음과 같은 메소드를 강제한다.

* boolean cancel : 말그대로 취소한다. 실행중일 경우 ```Interrupt```를 발생시킨다.
* boolean isCancelled : 취소되었는지 확인한다.
* boolean isDone : 끝났는지 확인한다.
* V get : 리턴값을 받아온다.
* V get(long timeout, TimeUnit unit) : 리턴값을 받아오는데 timeout시간이 지날 경우 ```TimeoutException```을 발생시킨다.

ExecutorService에서는 Future를 다루는 몇가지 메소드가 있는데 대표적으로 ```Callable```가 여러개일 경우 ```List```로 받아서 실행시키고 그 결과를 ```List<Future>```로 받거나 ```Runnable```을 실행하고 정상적으로 끝날 경우 지정한 ```T```를 반환하는 경우가 있다.

* <T> List<Future<T>> invokeAll(Collection<? extends Callable<T>> tasks)
* <T> List<Future<T>> invokeAll(Collection<? extends Callable<T>> tasks, long timeout, TimeUnit unit)
* <T> Future<T> submit(Callable<T> task)
* Future<?> submit(Runnable task) : 정상 종료되면  null을 반환한다.
* <T> Future<T> submit(Runnable task, T result)

### Future's implemeation


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTkxMzc1NjE5NCw4MDg1NDQ3NzgsLTEwMz
kzNDE1MDYsLTE5NTgwODg1NDJdfQ==
-->