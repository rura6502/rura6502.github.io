---
layout: post
title:  Java Queue Basic
date:   2019-05-12 00:00:00 +0900
categories: [Data-Structure, Queue, Java]
---

## Queue

First In First Out 방식의 자료구조. 데이터가 들어가는(입력) 큐의 입구는 tail/rear, 나오는 출구(출력)는 head/front 라고 부름.

> 예시. 음식점에 줄(Queue)은 먼저온 사람(First)이 제일 앞에 서서(In) 자리가 나면 제일 먼저(First) 먹을 수(Out) 있다. 

큐에 크기가 정해져 있느냐 없느냐에 따라 두 가지 용어가 사용됨.

* Unbounded Queue : 큐의 크기가 정해져 이

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

```deck``` 이라고도 부름. 큐의 양 끝(tail, head)에서 입력, 출력을 다 할 수 있음.

> 예시. 보통 사람들은 카드패를 돌릴 때 카드 더미에서 맨 위 혹은 맨 아래 카드를 준다. 위를 주던, 아래를 주던(head와 tail에서 출력이 가능) 주는 사람 마음이지만 굳이 번거롭게 중간에서 꺼내서 주진 않는다. 또한 상대방에게 카드를 받았을 때 카드 더미의 위에 얹거나 아래에 나두고 더미로 덮지(head와 tail에서의 입력) 일일이 중간에 끼워넣거나 하는 경우는 잘 없다.(약간의 억지...)

### BlokingQueue

큐에서 입/출력이 발생할 때 문제가 생기는 경우, 예를들어 입력 시 큐가 꽉 차있거나 출력 시 큐가 비어있을 때 쓰레드를 대기시켜서 해당 문제가 해결될 때까지 대기(Blocking) 시킴. 해결이란 아래를 의미. 무작정 기다리는 것을 방지하기 위해 별도의 timeout을 설정할 수도 있음.

* 출력이 대기 중일 경우 : 큐에 새로운 요소가 들어와서 '출력'이 가능해질 때 까지
* 입력이 대기 중인 경우 : 큐에 새로운 요소가 들어갈 자리가 생겨서 '입력'이 가능하질 때 까지

아래의 특징들이 있음

* null을 허용하지 않음. null을 넣으려고 하면 ```NullPointerException```이 발생함. null은 poll operation 시도 시 큐가 비어있을 때만 반환됨.
* bounded queue, 용량이 제한되어 있는 큐일 수 있음
* Tread-Safe. 큐에서 제공하는 모든 메소드는 lock을 사용하거나 currency control을 사용함. 
* Collection을 상속하였으므로 ```addAll, containsAll, retainAll``` 같은 ```bulk Collectio operation```을 사용할 수 있는데 안쓰는 것이 좋음. 작업중 일부가 실패할 수 있음.

### BlokingDequeue

BlokingQueue와 Dequeue를 상속하고 있음. 둘의 특성을 합친 것.

### TransferQueue

BlokingQueue를 상속하고 있음. 생산자(큐에 데이터를 넣는 측, Producer)는 소비자(큐에서 데이터를 가져가는 측, Consumer)가 엘리먼트를 가져갈 때 까지 기다림.

## refer to
[Deque (Java Platform SE 8)](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html)
[TransferQueue (Java Platform SE 8)](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/TransferQueue.html)
[BlokingQueue (Java Platform SE 8)](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/BlockingDeque.html)
[BlokingDeQueue (Java Platform SE 8)](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/BlockingDeque.html)
[Why do we need Deque data structures in the real world? [closed]](https://stackoverflow.com/questions/3880254/why-do-we-need-deque-data-structures-in-the-real-world)
<!--stackedit_data:
eyJoaXN0b3J5IjpbODIzMDE0MTcyXX0=
-->