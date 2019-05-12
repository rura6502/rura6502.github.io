---
layout: post
title:  Java Executor, ExecutorService
date:   2019-05-12 00:00:00 +0900
categories: [Data-Structure, Queue, Java]
---

## Queue
First In First Out 방식의 자료구조. 데이터가 들어가는(입력) 큐의 입구는 tail, 나오는 출구(출력)는 head라고 부름.

## Java Queue
자바에서는 ```java.util.Queue``` 인터페이스에 큐를 정의해 놓았음. 메소드로는 4가지가 추상화 정의되어 있음.

* boolean add(E e) : 큐에 새로운 엘리먼트를 추가. 큐에 추가될 여유 사이즈가 없으면 ```IllegalStateException```을 발생시킴.
* boolean offer(E e) : ```add```와 매우 비슷하게 동작하나 다른점은 큐에 사이즈가 없을 경우 ```False```를 반환.
* E remove() : head에서 요소를 하나를 반환함. 큐에 꺼낼 엘리먼트가 없을 경우 ```NoSuchElementException```를 던짐.
* E pool() : 


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4MTI3MDI0ODIsNTc0NjQwODgsLTU2OD
QyOTUxNl19
-->