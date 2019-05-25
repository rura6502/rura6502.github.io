---
layout: post
title:  Spring Boot Multi Module Proejct with Advaned Gradle
date:  2019-05-25 00:00:00 +0900
categories: [Spring_Boot, Gradle, Multi_Module]
---

> 본 포스팅에서 고안한 '유연하다'는 방식은 그루비 언어를 활용하여 구성하는 방식으로 정답은 아니고 더 좋은 방식이 있다면 조언바랍니다.

여러 연관성 있는 프로젝트를 한꺼번에 묶어서 관리하게 해주는 프로젝트를 멀티 모듈 프로젝트라고 함. 멀티 모듈 프로젝트를 구성하여 서브 프로젝트간에 공통 라이브러리를 공유하고 관리를 더 쉽게 할 수 있음.

![reason_of_using_multi_module_project](https://github.com/rura6502/rura6502.github.io/blob/master/_posts/_image/reason_of_using_multi_module_project.jpg?raw=true)

위에 있는 그림은 수많은 멀티모듈 프로젝트의  활용가능한 예시 중 하나이며 매우 다양하게 활용될 수 있음. 이렇게 뎁스가 2정도 되는 모듈에 대한 예제는 많으므로 뎁스가 3이면서 좀 더 유연하게 활용할 수 있는 멀티모듈 프로젝트를 구성 해 보겠음.

![gradle-flexible-multimodule-project-possible-example1](![gradle-flexible-multimodule-project-possible-example1.jpg](https://github.com/rura6502/rura6502.github.io/blob/master/_posts/_image/gradle-flexible-multimodule-project-possible-example1.jpg?raw=true)

gradle은 `build.gradle`, `settings.gradle` 파일을 사용하여 프로젝트를 구성하게 되는데 이 파일들은 groovy라는 `프로그래밍 언어`를 사용한다는 점과 어느정도 자바 라이브러리를 지원한다는 아이디어를 착안하여 몇가지 룰을 정하고 그 룰에 맞게 라이브러리를 주입하는 방식을 구상하였음. 해야 할 일은 다음과 같음.

1. 트리형 구조를 가지는 프로젝트 구조를 구성
2. 프로젝트 이름에 몇가지 룰을 적용하여 서브 프로젝트의 이름을 구성
3. 기본적인 자바 소스 파일 구조(src.main.java.그룹.아티팩트, src.main.resource.그룹.아티팩트)를 자동으로 생성

아래 기술할 프로젝트의 구조와 설계할 조건은 아래와 같음

* 그룹아이디는 `io.github.rura6502`이다.
* 웹 프로젝트는 이름이 `_web` 으로 끝난다.
* `_web`으로 끝나는 프로젝트는 기본적으로 모두 `spring boot web`를 사용한다.
* 하위 프로젝트 간에 공통 모듈의 이름은 `*_comm`으로 끝난다.
* 모든 프로젝트는 `lombok`을 사용한다.
* 자식이 있는 프로젝트는 그 프로젝트 자체가 소스를 가지진 않고 묶음으로써의 기능만 한다.

![gradle-flexible-multimodule-project-possible-example2.jpg](https://github.com/rura6502/rura6502.github.io/blob/master/_posts/_image/gradle-flexible-multimodule-project-possible-example2.jpg?raw=true)

`settings.gradle` 파일에는 프로젝트의 전체 구조(`부모:자식`) 관계를 구성함.

```groovy
rootProject.name="A"

include "A_comm"
include "A_web_comm"
// // for A_a
include "A_a:A_a_web"
include "A_a:A_a_comm"
include "A_a:A_a_b"
// for A_b
include "A_b:A_b_web"
include "A_b:A_b_comm"
include "A_b:A_b_b"
```

`gradle.build`파일에서 앞서 기술한 다양한 빌드 설정들을 해주어야 함. 우선 빌드 행위들을 적절한 위치에 해주기 위해선 그래들에서 원하는 구조를 이해를 하고 내가 원하는 행위가 해당하는 구조에 들어가함. 그래들에선 이 구조를 `Build script structure`라는 이름으로 도큐먼트상에 설명함.

다양한 스트럭쳐 중 우리가 주로 활용해야 할 것은 `subprojects` 임. 해당 프로제트는 현재 구성되어 있는 모든 서브 프로젝트에 이 설정이 적용되게 되어있는데 여기서 말하는 `모든`이라는 말은 한꺼번에 적용 된다는게 아니라 프로젝트를 순회하며 적용함. 마치 `for`문을 사용하여 적용함. ```build.gradle``에 아래 코드만 적고 실행하면 어떻게 동작하는지 알수 있음.
```groovy
// build.gradle
subprojects {
  System.out.println("2------")
  System.out.println(project.name)
}

// 결과
2------
A_a
2------
A_b
2------
A_comm
2------
A_web_comm
2------
A_a_b
2------
A_a_comm
2------
A_a_web
2------
A_b_b
2------
A_b_comm
2------
A_b_web
```

이를 이용하면 앞서 사전에 정의하였던 `프로젝트 네이밍 규칙`과 `gradle은 Programming Language인 groovy로 구성'이라는 점을 이용하여 아래와 같이 특정 조건에 부합하는 프로젝트엔 특정 디펜던시를 주입하거나 특정 프로젝트를 참조하도록 할 수 있음.

```groovy
subproejcts {
  if  (프로젝트 이름이 '_web'으로 끝나면) {
    do 디펜던시 spring-boot-stareter-web 을 주입
  }
}
```

일단 기본적인 전체 구조부터 설정

```groovy
buildscript {
  ext {
    springBootVersion = '2.1.5.RELEASE' // 개별 
  }

  repositories {
    mavenCentral()
  }
  dependencies {
    classpath "org.springframework.boot:spring-boot-gradle-plugin:${springBootVersion}"
    classpath "io.spring.gradle:dependency-management-plugin:0.6.0.RELEASE"
  }
}
```

아래부턴 `subprojects` 블록의 내용

## refer to
[Gradle Build Language  Reference](https://docs.gradle.org/current/dsl/index.html)





<!--stackedit_data:
eyJoaXN0b3J5IjpbOTQ1ODgwMTEsMTc3NDc1MDE1LC05NDM3MD
IwMTYsMTI0OTUyNjUyNSw1MDk2MjY4NjEsMTcxNDQ4ODE3Nywt
MTg1MDU3NzI3OCwtMTk5MTQ1MDUwMF19
-->