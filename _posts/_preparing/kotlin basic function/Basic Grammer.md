---
layout: post
title:  Kotlin Basic Grammer
date:  2019-05-11 00:00:00 +0900
categories: [Kotlin, Basic, Language]
---

## Hello World

```kotlin
// 패키지 이름, 명시하지 않으면 default package로 지정됨.
package org.kotlinlang.play

// Kotlin application의 entry point. Java의 main 함수. Kotlin 1.3부터는 파라미터 없이 선언할 수 있음. 이전 버전은 'args: Array<String>'을 적어야 함.
fun main() {
  println("Hello, World!") // 표준 출력. 문장 끝의 세미콜론(;)은 optional.
}
```

### Functions

```kotlin
function printMessage(message: String): Unit {
  println(message)
}
// String 파라미터가 하나있고 Unit 타입을 반환하는 함수. 현재 return은 null 임.


```





## refer to
[Learn Kotlin by Example](https://play.kotlinlang.org/byExample/overview)


<!--stackedit_data:
eyJoaXN0b3J5IjpbODcxMTU4MTksLTE1NDc0Nzk1OTFdfQ==
-->