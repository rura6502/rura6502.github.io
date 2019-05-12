---
layout: post
title:  Java Thread Callable & Future
date:  2019-05-11 00:00:00 +0900
categories: [Thread, Java, Spring, Spring Boot]
---

## Callable

자바의 쓰레드는 별도의 반환값(return)이 없다. 

```java
public interface Runnable {
    public abstract void run();  // 반환이 void 이다.
}
```

하지
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDk5MzMwMjYyLC0xOTU4MDg4NTQyXX0=
-->