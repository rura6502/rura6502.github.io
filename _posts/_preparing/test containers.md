---  
layout: post  
title:  Test Container for Database
date:  2019-06-21 00:00:00 +0900  
categories: [testcontainers]
---
# Test Containers
애플리케이션의 기능을 테스트할 때 데이터베이스나 다른 여러 외부요소들과 엮인 기능의 경우 JUnit등의 테스트 프레임워크를 사용해서 테스트를 하기가 곤란한 경우가 많다.  어떠한 테스트를 수행할 때 상황에 따라 성공여부가 나뉜다면 전체 애플리케이션 테스트가 매우 번거로울 수 있다. 예를들어 내 컴퓨터에서 테스트를 했을 때 모두 Pass했지만 다른 컴퓨터에서는 Fail되는 경우이다.  특히 애플리케이션 본체가 아닌 다른 외부 요소들과 엮일 경우 이러한 현상이 매우 빈번하게 발생할 수 있다.

# For Database
Spring의 경우에 관계형 데이터베이스의 경우 추상화환 ORM 프레임워크인 JPA 등을 사용해서 QueryDSL, JPQL, Query Method 등으로 쿼리를 구성하기 때문에 특정 데이터베이스에 대한 종속을 벗어날 수 있고, 이를 테스트하기 위해 H2 inmemory db를 사용해서 간단하게 테스트할 수 있도록 제공하고 있다. 또한 관계형 데이터베이스 뿐만 아니라 Radis, MongoDB등의 테스트를 위해서 테스트 환경에서만 잠깐 구동해볼 수 있는 Embedded 시리즈들을 많이 제공한다.

애플리케이션을 테스트하다 보면 특정 데이터베이스나 시스템을 꼭 사용해서 테스트해야 하는 경우가 생기는데 예를들어 애플리케이션이 특정 벤더에서 제공하는 시스템의 함수나 API등을 사용하는 경우이다. 이러한 경우는 대게 Mock객체를 이용해서 임의로 만들어주거나 혹은 테스트용 API를 실제로 구현해서 수동으로 테스트하는 수 밖에 없었다.

Testcontainers(https://www.testcontainers.org/)는 de facto로 사용되는 도커를 사용해서 필요로하는 특정 시스템의 이미지를 테스트시에 구동시키고 테스트가 종료되면 지워주는 라이브러리이다. 예를들어 



# refer to
[Testcontainers official site](https://www.testcontainers.org/)
[Finally, easy integration testing with Testcontainers](https://www.slideshare.net/rdebusscher/finally-easy-integration-testing-with-testcontainers)


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwMzU5MDgzMzMsMTM0MTg3MDE0OSwtMz
AzODg3Mjg0LC05NTM5MzExODQsLTEwODM1NTY5ODYsMTMyMzUw
NDQzMV19
-->