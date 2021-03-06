---
layout: post
autor: rura6502
title:  Java Thread Basic
date:  2019-05-11 00:00:00 +000
categories: [Language, Java]
tags: [Jjava hread]
math: true
---

## 쓰레드(Thread)란

하나의 프로세스 내부에서 각각의 독립된 흐름.

프로세스 : 프로그램을 실행시키면 OS는 컴퓨팅 자원(CPU, Memory)를 할당하여 인스턴스를 생성하여 프로그램을 구동하게 되는데 여기서 ***컴퓨팅 리소스를 할당받아 수행되는 인스턴스***를 프로세스라고 함. 각각의 프로세스는 독자적인 메모리 공간을 사용하여 ***서로간의 공간에 접근할 수 없음***(다른 프로세스의 변수에 접근할 수 없다). 프로세스간 통신을 위해선 별도의 통신방식(socket, IPC, http)이 필요.

쓰레드 : 프로세스 내에서 실행되는 일련의 작업의 기본 단위. 한 프로세스는 적어도 하나 이상의 쓰레드(메인 쓰레드)를 가짐. 같은 프로세스 내부에서 실행되는 쓰레드간에는 공유 영역을 통하여 변수 등을 공유할 수 있음.(공유로 인하여 많은 문제점들이 발생). 쓰레드를 생성하여 어떠한 작업을 할 때 이를 태스크(Task)라고 부르기도 함.

> 은행->프로세스, 창구직원->쓰레드, 고객->작업, 업무장비->공유영역
> **쓰레드가 한개인 경우** : 은행에 창구직원이 한명이면 고객들은 줄서서 앞에 있는 고객의 업무가 모두 끝날 때 까지 기다려야한다.
> **다중 쓰레드인 경우** : 창구가 여러개라면 여러명의 고객들을 한꺼번에 개별업무로 처리가 가능하다.
> **같은 프로세스의 쓰레드 간 공유 영역 사용** : 은행창구 직원들은 필요할 경우 프린터, 스캐너, 전체 장부(은행업무 프로그램)등을 공유하여 같이 사용한다.

## Java Thread

  자바의 쓰레드는 ```java.lang.Thread```를 상속받아서 쓰레드로 실행할 부분을 ```run()``` 메소드에 적어서 오버라이드함. ```java.lang.Runnable``` 클래스를 구현하는 방법도 있음.

```java
public class Basic extends Thread {
  String i = null;  // #1 : 개별 쓰레드가 각자의 영역에서 사용할 변수 선언
  public void run() {  // #2 : 쓰레드를 사용하여 개별로 실행될 영역 구현
    i = Thread.currentThread().getName();  // 추후 테스트를 위한 코드
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
      new Basic().setThreadName("T" + i).start();  // #4 : 총 9개의 쓰레드를 생성하여 실행
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

  쓰레드를 쓰지 않은 채로 구현한다면 T0~9까지 차례대로 나와야 하지만 쓰레드를 사용하였으므로 ```System.out.println(이하 출력메소드)``` 메소드는 T0~9 까지의 별개의 쓰레드로 실행되었음. 그리고 쓰레드는 한 프로세스 내부에서 개별로 실행되므로 출력메소드를 먼저 처리하는 쓰레드가 먼저 출력해버려서 위와 같은 결과가 출력.

하지만 i가 여러 쓰레드가 공유하는 영역의 변수일 경우 문제가 발생할 수 있음.

> 창구가 여러개인 은행에서 나는 옆 창구의 고객이 말, 행동등이 느리거나 시간이 오래걸리는 업무를 기다릴 필요가 없다. 내 창구의 은행직원은 내 업무만 처리하면 되고, 나는 내 업무만 끝나면 옆창구에 사람이 앉아있던 업무가 오래걸리던 상관없이 그냥 끝내고 나가면 된다.

### Thread를 사용할 때 발생할 수 있는 문제점

#### 공유 영역(변수) 사용의 문제

  같은 프로세스의 쓰레드 간에는 일정 공유영역을 이용할 수 있지만 이런 기능으로 여러가지 문제가 발생할 수 있음. 특정 쓰레드가 공유 영역의 변수 a를 변경하였을 때 다른 쓰레드는 변경 전의 a의 값을 이미 이용하고 잘못된 연산 값을 덮어쓸 수 있게 됨.

```java
public class Basic extends Thread {
  static String i = null;  // #1 : 공유 영역으로 사용하기 위하여 static 키워드 사용
  public void run() {
    // 모든 쓰레드가 같은 공용 변수에 접근하여 값을 변경
    Basic.i = Thread.currentThread().getName();
    // 간섭을 극대화 하기 위하여 0.1 초 동안 sleeop
    try {
      TimeUnit.MILLISECONDS.sleep(100);
    } catch (InterruptedException e) {
      // TODO Auto-generated catch block
      e.printStackTrace();
    }
    // 이 변수는 쓰레드 개별공간의 변수
    String currentThreadName = Thread.currentThread().getName();  
    // 위에서 변경한 공용 공간의 변수와 개별 공간의 변수를 출력, 비교
    System.out.println(i
                       + ", " + Thread.currentThread().getName()
                       + ", " + currentThreadName.equals(i));  
  }
  public Basic setThreadName(String name) {...} // 생략
  public static void main(String[] args) {...} // 생략
}

// 결과
T9, T9, true
T9, T2, false
T9, T6, false
T9, T3, false
T9, T4, false
T9, T7, false
T9, T1, false
T9, T0, false
T9, T5, false
T9, T8, false
```

  첫번째 결과와는 다르게 공유 영역에 있는 i는 생성한 모든 쓰레드가 간섭하여 각자 입력받은 i의 값을 덮어씀으로써 예상치 못한 결과가 발생하였음. 이렇게 쓰레드간 공유가능한 변수에 여러 쓰레드가 접근하며 각자가 인지한 값과 계산한 값을 업데이트 함으로써 예상치 못한 결과를 얻을 수 있음.

> 은행에서는 항상 은행의 현금이 총 얼마 있는지 단 한개의 공용장부에 실시간으로 기재해야 한다. 현재 현금이 100 만큼 있으며 공용 장부에도 100 이라고 적혀있다. 공용 장부는 각각 창구의 개별 컴퓨터에서 창구 직원이 업데이트할 수 있다.
> 고객이 현금을 찾거나 맡길 때 아래와 같은 과정을 거친다.
> 1. 고객이 현금을 찾거나 맡기기를 요청한다.
> 1. 직원은 공용장부를 확인한다.
> 1. 직원은 현금을 고객에게 주거나 받는다.
> 1. 직원은 공용장부를 업데이트한다.
> 
> 발생할 수 있는 문제 상황
> 1. [실제현금 100, 공용장부 100] : 고객A가 직원1에게 현금 10을 요청한다.
> 1. [실제현금 100, 공용장부 100] : 직원1은 공용장부의 총 현금 100을 확인한다.
> 1. [실제현금 100, 공용장부 100] : 고객B가 직원2에게 현금 20을 요청한다.
> 1. [실제현금 100, 공용장부 100] : 직원2는 공용장부의 총 현금 100을 확인한다.
> 1. [실제현금 90, 공용장부 100] : 직원1은 현금 10을 고객A에게 준다.
> 1. [실제현금 90, 공용장부 90] : 직원1은 자기가 확인한 공용장부의 총 현금이 100이였고 자기가 10을 내어줬으므로 공용장부를 90으로 업데이트한다.
> 1. [실제현금 70, 공용장부 90] : 직원2는 현금 20을 고객B에게 준다.
> 1. [실제현금 70, 공용장부 80] : 직원2는 자기가 확인한 공용장부의 총 현금이 100이였고 자기가 20을 내어줬으므로 공용장부에 80으로 업데이트 한다.
> * 실제 현금은 70이나 공용장부에 80이라고 적혀버리는 문제가 발생하였다.

  이런 예상치 못한 결과를 막기위해서 쓰레드간 공용으로 사용하는 변수를 ```synchronized``` 키워드를 사용하여 일종의 '잠금' 형식으로 사용할 수 있음.

```java
public class Basic extends Thread {
  static String i = null;
  public void run() {
    synchronized (Basic.class) {  // 
      i = Thread.currentThread().getName();
      try {
        TimeUnit.MILLISECONDS.sleep(100);
      } catch (InterruptedException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
      }
      String currentThreadName = Thread.currentThread().getName();
      System.out.println(i
                                      + ", " + Thread.currentThread().getName()
                                      + ", " + currentThreadName.equals(i));
  }
  public Basic setThreadName(String name) {...} // 생략
  public static void main(String[] args) {...} // 생략
}

// 결과
T0, T0, true
T2, T2, true
T8, T8, true
T7, T7, true
T9, T9, true
T5, T5, true
T4, T4, true
T3, T3, true
T6, T6, true
T1, T1, true
```

  ```synchronized``` 블록을 사용하여 쓰레드가 공유 변수에 접근하여 변경하고 출력하기까지의 과정에 lock(자물쇄)를 걸어 어떤 쓰레드가 ```synchronized``` 안의 블록을 실행하는 동안, 다른 쓰레드는 해당 블록을 수행하지 않고 종료될 때 까지 기다렸다가 수행되도록 하였음. 이렇게 하여 특정 쓰레드가 실행 되는 동안 다른 쓰레드가 공유 변수에 접근하여 값을 변경하는 예측할 수 없는 상황을 방지할 수 있음.

> 은행 공용장부를 각 창구의 개별 컴퓨터에서 업데이트할 수 없게 하고 단 한개 있는 공용장부를 사용하려면 직원이 가져가게 함. 그럼 다른 직원은 공용장부를 보거나 업데이트할 수 없고 공용장부를 보고 써야하는 자기의 업무는 다른 직원의 공용장부 업무가 끝날 때 까지 기다려야 함.
> 1. [실제현금 100, 공용장부 100] : 고객1은 직원1에게 10을 요청한다.
> 1. [실제현금 100, 공용장부 100] : 직원1은 공용장부를 가져온다.
> 1. [실제현금 100, 공용장부 100] : 고객2은 직원2에게 20을 요청한다.
> 1. [실제현금 100, 공용장부 100] : 직원2는 공용장부를 직원1이 가져갔으므로 직원1이 공용장부를 제자리에 갔다놓을 때 까지 기다린다.
> 1. [실제현금 90, 공용장부 90] : 직원1은 고객1에게 10을 주고 공용장부에 90을 적는다. 
> 1. [실제현금 90, 공용장부 90] : 직원1은 공용 장부를 제자리에 가져다 놓는다.
> 1. [실제현금 70, 공용장부 70] : 직원2는 공용장부를 가져와서 고객2에게 20을 주고 공용장부에 마지막 금액에서 -20을 한 금액인 70을 적는다.

  해당 코드를 실행해보면 알겠지만 이전의 예제와는 다르게 출력에 딜레이가 생김. ```synchronized``` 블록은 특정 쓰레드가 해당 구역을 실행 중이면 다른 쓰레드는 사용하고 있는 쓰레드가 블록을 다 실행할 때 까지 기다렸다가 종료되고 나면 실행하기 때문임. 이러한 방식은 실제 환경에서 로직의 딜레이를 발생시킬 수 있으므로 신중히 고려하여 사용하여야 함.

> 여기서 사용한 ```synchronized``` 키워드 사용 방법은 쓰레드가 공유 변수에 접근하면서 생길 수 있는 문제점과 그 문제점을 방지하기 위해 간략하게 든 예시

#### 메모리 사용의 문제

쓰레드는 프로세스 내부에 실행되어 프로세스가 실행되는 만큼의 오버헤드(부하)를 컴퓨팅 리소스에 주진 않지만 그렇다고 무작정 생성할 경우 운영중인 시스템에서 치명적인 컴퓨팅 리소스 오버 상태를 야기하여 전체 프로그램의 성능을 저하시키거나 불능 상태에 빠질 수 있음.

> 고객이 늘어나는 만큼 추후를 생각하지 않고 은행 창구를 늘리고 직원을 새로 고용하면 은행은 결국 파산할 것이다.

이를 해결하기 위한 방법은, 요청을 적절히 처리할 수 있는 적당한 양의 쓰레드를 미리 만들어 놓고 이를 관리하면서 필요할 경우 사용하고 모두 사용 중일 경우 대기시킨 다음 끝난 쓰레드를 다시 준비시켜 동작시키는 방식을 사용할 수 있음. 이렇게 무엇인가 필요한 것을 ***미리 만들어 놓고 요청에 바로 응답***하며 끝난 것을 다시 관리하고 내어주는 것을 ```pool``` 방식이라고 하며 쓰레드를 ```pool``` 방식으로 관리하는 것을 ```쓰레드 풀(Thread Pool)``` 이라고 칭함.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExNzY5OTM4MDMsLTE2NzQ1NzQ4NDcsLT
EyNDc3NzQ4MjMsMTIyMDA3MDA4MywxNjgyMjM2NTI1LC0xODUx
MDgxODkwXX0=
-->