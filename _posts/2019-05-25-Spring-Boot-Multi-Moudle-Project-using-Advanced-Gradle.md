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

![gradle-flexible-multimodule-project-possible-example](https://github.com/rura6502/rura6502.github.io/blob/master/_posts/_image/gradle-flexible-multimodule-project.jpg?raw=true)

gradle은 `build.gradle`, `settings.gradle` 파일을 사용하여 프로젝트를 구성하게 되는데 이 파일들은 groovy라는 `프로그래밍 언어`를 사용한다는 점과 어느정도 자바 라이브러리를 지원한다는 아이디어를 착안하여 몇가지 룰을 정하고 그 룰에 맞게 라이브러리를 주입하는 방식을 구상하였음. 해야 할 일은 다음과 같음.

1. 트리형 구조를 가지는 프로젝트 구조를 구성
2. 프로젝트 이름에 몇가지 룰을 적용하여 서브 프로젝트의 이름을 구성
3. 기본적인 자바 소스 파일 구조(src.main.java.그룹.아티팩트, src.main.resource.그룹.아티팩트)를 자동으로 생성







<!--stackedit_data:
eyJoaXN0b3J5IjpbMTcxNDQ4ODE3NywtMTg1MDU3NzI3OCwtMT
k5MTQ1MDUwMF19
-->