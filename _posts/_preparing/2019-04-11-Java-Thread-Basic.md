---
layout: post
title:  Java Thread Basic
date:   today
categories: [java]
---

# 쓰레드(Thread) 란?
하나의 프로세스 내부에서 각각의 독립된 흐름.

프로세스 : 프로그램을 실행시키기 위하여 CPU의 시간, 메모리의 공간등 ***컴퓨팅 리소스를 할당받아 수행되는 작업의 단위***. 각각의 프로세스는 독자적인 메모리 공간을 사용하여 ***서로간의 공간에 접근할 수 없음***(다른 프로세스의 변수에 접근할 수 없다). 프로세스간 통신을 위해선 별도의 통신방식(socket, IPC, http)이 필요.

쓰레드 : 프로세스 내에서 실행되는 흐름의 기본 단위. 한 프로세스는 적어도 하나 이상의 쓰레드(메인 쓰레드)를 가짐. 한 프로세스 내부에서 실행되는 쓰레드간에는 공유 영역을 통하여 변수 등을 공유할 수 있음.(공유로 인하여 많은 문제점들이 발생)

> 예시 1. 은행->프로세스, 창구직원->쓰레드, 고객->작업, 업무장비->공유영역
> **쓰레드가 한개인 경우** : 은행에 창구직원이 한명이면 고객들은 줄서서 앞에 있는 고객의 업무가 모두 끝날 때 까지 기다려야한다.
> **다중 쓰레드인 경우** : 창구가 여러개라면 여러명의 고객들을 한꺼번에 개별업무로 처리가 가능하다.
> **한 프로세스의 쓰레드 간 공유 영역 사용** : 은행창구 직원들은 필요할 경우 프린터, 스캐너, 전체 장부(은행업무 프로그램)등을 공유하여 같이 사용한다.

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
   new Basic(i).start();
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

Thread를 쓰지 않은 채로 구현한다면 0~9까지 차례대로 나와야 하지만 쓰레드를 사용하여 

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTM2OTU0NDM2MywtMTI1MjM2MDAwMSwtMj
A4ODc2NTczLC0yMDIwODY2NTExXX0=
-->