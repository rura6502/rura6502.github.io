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
  String i = null;  // #1 : 개별 쓰레드가 각자의 영역에서 사용할 변수 선언
  public void run() {  // #2 : 쓰레드를 사용하여 개별로 실행될 영역 구현
    i = Thread.currentThread().getName();
    String currentThreadName = Thread.currentThread().getName();
    System.out.println(i
                       + ", " + Thread.currentThread().getName()
                       + ", " + currentThreadName.equals(i));  
  }
  public Basic setThreadName(String name) {
    // #3 : 테스트를 위하여 이름 지정 메소드 구현 
    super.setName(name);
    return this;
  }
  public static void main(String[] args) {
    // Basic basic = new Basic();
    for (int i = 0; i < 10; i++) {
      new Basic().setThreadName("T" + i).start();  // #4 : 쓰
    }
  }
}

// 결과
T0, T0, true
T1, T1, true
T2, T2, true
T5, T5, true
T8, T8, true
T6, T6, true
T7, T7, true
T4, T4, true
T3, T3, true
T9, T9, true
```

쓰레드를 쓰지 않은 채로 구현한다면 T0~9까지 차례대로 나와야 하지만 쓰레드를 사용하였으므로 ```System.out.println(이하 출력메소드)``` 메소드는 T0~9 까지의 별개의 쓰레드로 실행되었다. 그리고 쓰레드는 한 프로세스 내부에서 개별로 실행되므로 출력메소드를 먼저 처리하는 쓰레드가 먼저 출력해버려서 위와 같은 결과가 출력되었다.

```java
Thread[Thread-1,5,main] @ 9
Thread[Thread-9,5,main] @ 9
```

하지만 i가 여러 쓰레드가 공유하는 영역의 변수일 경우 문제가 발생한다.

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

첫번째 결과와는 다르게 공유 영역에 있는 i는 생성한 모든 쓰레드가 간섭하여 각자 입력받은 i의 갚을 덮어씀으로써 예상치 못한 결과가 발생하였다. 이렇게 쓰레드간 공유가능한 변수에 여러 쓰레드가 접근하며 각자가 인지한 값과 계산한 값을 업데이트 함으로써 예상치 못한 결과를 얻을 수 있다.

> 예시. 은행에서는 항상 은행의 현금이 총 얼마 있는지 공용장부에 실시간으로 기재해야 한다. 현재 현금이 100 만큼 있으며 공용 장부에도 100 이라고 적혀있다. 공용 장부는 각각 창구의 개별 컴퓨터에서 창구 직원이 업데이트할 수 있다.
> 고객이 현금을 찾거나 맡길 때 아래와 같은 과정을 거친다.
> 1. 고객이 현금을 찾거나 맞기기를 요청한다.
> 1. 직원은 공용장부를 확인한다.
> 1. 직원은 현금을 고객에게 주거나 받는다.
> 1. 직원은 공용장부를 업데이트한다.
> 
> 발생할 수 있는 문제 상황
> 1. [실제현금 100, 공용장부 100] : 고객A가 직원1에게 현금 10을 요청한다.
> 1. [실제현금 100, 공용장부 100] : 직원1은 공용장부의 총 현금 100을 확인한다.
> 1. [실제현금 100, 공용장부 100] : 고객B가 직원2에게 현금 20을 요청한다.
> 1. [실제현금 100, 공용장부 100] : 직원2는 공용장부의 총 현금 100을 확인한다.
> 1. [실제현금   90, 공용장부 100] : 직원1은 현금 10을 고객A에게 준다.
> 1. [실제현금   90, 공용장부   90] : 직원1은 자기가 확인한 공용장부의 총 현금이 100이였고 자기가 10을 내어줬으므로 공용장부를 90으로 업데이트한다.
> 1. [실제현금   70, 공용장부   90] : 직원2는 현금 20을 고객B에게 준다.
> 1. [실제현금   70, 공용장부   80] : 직원2는 자기가 확인한 공용장부의 총 현금이 100이였고 자기가 20을 내어줬으므로 공용장부에 80으로 업데이트 한다.
> * 실제 현금은 70이나 공용장부에 80이라고 적혀버리는 문제가 발생하였다.

이런 예상치 못한 결과를 막기위해서 쓰레드간 공용으로 사용하는 변수를 ```synchronized``` 키워드를 사용하여 일종의 '잠금' 형식으로 사용할 수 있다.


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwMTIzMTY2NzMsMTQxODY3Mzk5NiwtMT
Q1MjUwODA1MSwyNjQxNzg0ODEsLTE5NjY5MjI3MjMsLTEyNTIz
NjAwMDEsLTIwODg3NjU3MywtMjAyMDg2NjUxMV19
-->