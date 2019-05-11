---
layout: post
title:  Java Thread Basic
date:   today
categories: [java]
---

# 쓰레드(Thread) 란?
하나의 프로세스 내부에서 각각의 독립된 흐름.

프로세스 : 프로그램을 실행시키기 위하여 CPU의 시간, 메모리의 공간등 ***컴퓨팅 리소스를 할당받아 수행되는 작업의 단위***. 각각의 프로세스는 독자적인 메모리 공간을 사용하여 ***서로간의 공간에 접근할 수 없음***(다른 프로세스의 변수에 접근할 수 없다). 프로세스간 통신을 위해선 별도의 통신방식(socket, IPC, http)이 필요.

쓰레드 : 프로세스 내에서 실행되는 흐름의 기본 단위. 한 프로세스는 적어도 하나 이상의 쓰레드(메인 쓰레드)를 가짐. 같은 프로세스 내부에서 실행되는 쓰레드간에는 공유 영역을 통하여 변수 등을 공유할 수 있음.(공유로 인하여 많은 문제점들이 발생)

> 예시. 은행->프로세스, 창구직원->쓰레드, 고객->작업, 업무장비->공유영역
> **쓰레드가 한개인 경우** : 은행에 창구직원이 한명이면 고객들은 줄서서 앞에 있는 고객의 업무가 모두 끝날 때 까지 기다려야한다.
> **다중 쓰레드인 경우** : 창구가 여러개라면 여러명의 고객들을 한꺼번에 개별업무로 처리가 가능하다.
> **같은 프로세스의 쓰레드 간 공유 영역 사용** : 은행창구 직원들은 필요할 경우 프린터, 스캐너, 전체 장부(은행업무 프로그램)등을 공유하여 같이 사용한다.

## Java Thread

자바의 쓰레드는 ```java.lang.Thread```를 상속받아서 쓰레드로 실행할 부분을 ```run()``` 메소드에 적어서 오버라이드한다. ```java.lang.Runnable``` 클래스를 구현해도된다.

```java
public class Basic extends Thread {
  int i
  Basic(int i) {
    this.i = i;
  }
  public void run() {
    // 현재 문장을 실행하는 쓰레드의 정보를 출력한다.
    System.out.println(Thread.currentThread() + " @ " + i);
  }
  public static void main(String[] args) {
    for (int i = 0; i < 10; i++)
      new Basic(i).start(); // 쓰레드를 시작한다.
  }
}

// 결과
Thread[Thread-0,5,main] @ 0
Thread[Thread-6,5,main] @ 6
Thread[Thread-1,5,main] @ 1
Thread[Thread-8,5,main] @ 8
Thread[Thread-9,5,main] @ 9
Thread[Thread-2,5,main] @ 2
Thread[Thread-3,5,main] @ 3
Thread[Thread-4,5,main] @ 4
Thread[Thread-5,5,main] @ 5
Thread[Thread-7,5,main] @ 7
```

쓰레드를 쓰지 않은 채로 구현한다면 0~9까지 차례대로 나와야 하지만 쓰레드를 사용하여 각각의 개별 쓰레드로 실행되어 먼저 처리되는 것(먼저 ```System.out.println```이 수행되는)이 먼저 나와서 순차적으로 되지 않았다. 하지만 쓰레드의 번호와 변수 i의 값은 똑같같다. 쓰레드가 수행하는 출력 메소드는 개별 속도에 따라 천차만별로 끝났지만 쓰레드가 생성하면서 할당받은 값은 각각 개별로 할당받아 다른 쓰레드에 영향을 받지 않고 정상적으로 출력되었다.

> 예시. 창구가 여러개인 은행에서 나는 옆 창구의 고객이 말, 행동등이 느리거나 시간이 오래걸리는 업무를 기다릴 필요가 없다. 내 창구의 은행직원은 내 업무만 처리하면 되고, 나는 내 업무만 끝나면 옆창구에 사람이 앉아있던 업무가 오래걸리던 상관없이 그냥 끝내고 나가면 된다.

### Thread를 사용할 때 발생할 수 있는 문제점

#### 공유 영역(변수) 사용의 문제

같은 프로세스의 쓰레드 간에는 일정 공유영역을 이용할 수 있지만 이런 기능으로 여러가지 문제가 발생할 수 있다. 특정 쓰레드가 공유 영역의 변수 a를 변경하였을 때 다른 쓰레드는 변경 전의 a의 값을 이미 이용하고 잘못된 연산 값을 덮어쓸 수 있다.

```java
public class Basic extends Thread {
  static int i = 0; // #1
  public void run() {
    System.out.println(Thread.currentThread() + " @ " + i);
  }
  public Basic setI(int i) {  // #2
    Basic.i = i;
    return this;
  }
  public static void main(String[] args) {
    for (int i=0; i<10; i++)
      new Basic().setI(i).start();
  }
}

// 결과
Thread[Thread-0,5,main] @ 5
Thread[Thread-5,5,main] @ 8
Thread[Thread-1,5,main] @ 7
Thread[Thread-4,5,main] @ 9
Thread[Thread-6,5,main] @ 9
Thread[Thread-7,5,main] @ 9
Thread[Thread-9,5,main] @ 9
Thread[Thread-8,5,main] @ 9
Thread[Thread-3,5,main] @ 9
Thread[Thread-2,5,main] @ 9
```

#1 : 모든 쓰레드가 공유할 수 있는 공유 변수를 설정하였다.
#2 : i를 설정할 수 있는 별도의 메소드를 만들었다.

첫번째 코드에서는 


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTY1ODEyNjk4NywyNjQxNzg0ODEsLTE5Nj
Y5MjI3MjMsLTEyNTIzNjAwMDEsLTIwODg3NjU3MywtMjAyMDg2
NjUxMV19
-->