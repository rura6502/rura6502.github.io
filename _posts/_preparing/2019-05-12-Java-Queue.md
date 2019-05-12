---
layout: post
title:  Java Executor, ExecutorService
date:   2019-05-12 00:00:00 +0900
categories: [Data-Structure, Queue, Java]
---

## Queue

First In First Out 방식의 자료구조. 데이터가 들어가는(입력) 큐의 입구는 tail/rear, 나오는 출구(출력)는 head/front 라고 부름.

> 예시. 음식점에 줄(Queue)은 먼저온 사람(First)이 제일 앞에 서서(In) 자리가 나면 제일 먼저(First) 먹을 수(Out) 있다. 

## Java Queue

자바에서는 ```java.util.Queue``` 인터페이스에 큐를 정의해 놓았음. Iterable, Collection을 상속하고 있어 순회가능한 컬렉션의 일종으로 보면 됨. 메소드로는 4가지가 추상화 정의되어 있음.

* boolean add(E e) : 큐에 새로운 엘리먼트를 추가. 큐에 추가될 여유 사이즈가 없으면 ```IllegalStateException```을 발생시킴.
* boolean offer(E e) : ```add```와 매우 비슷하게 동작하나 다른점은 큐에 사이즈가 없을 경우 ```False```를 반환. ```add```보다 더 좋다고 설명함.
* E remove() : head에서 요소를 하나를 반환함. 큐에 꺼낼 엘리먼트가 없을 경우 ```NoSuchElementException```를 던짐.
* E pool() : ```remove```와 유사하게 동작하나 큐에 꺼낼 엘리먼트가 없을 경우 ```null```을 반환함.
* E element() : 큐에서 현재 head에 있는 엘리먼트를 반환하는데 ```remove```나 ```pool``` 처럼 지우진 않고 그냥 가져옴. 가져올 것이 없으면 ```NoSuchElementException```
* E peek() : ```element```와 같으나 가져올 것이 없으면 ```null```을 반환함.

## Java Queue Subinterface

크게 4가지의 특성에 따라 Subinterface를 구현해놓고 이 4가지 특성을 따르는 많은 구현체들이 있음.

### Deque(Dobule-Ended Queue)

```deck``` 이라고도 부름. 

### BlokingQueue



* BlockingDequeue : 
* BlockingQueue :
* Deque :
* TrasferQueue : 


## refer to
[Deque (Java Platform SE 8](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html)]
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzQzMTIwNTA2XX0=
-->