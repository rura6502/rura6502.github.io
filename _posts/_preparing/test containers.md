---  
layout: post  
title:  Test Container를 사용한 테스트
date:  2019-06-21 00:00:00 +0900  
categories: [testcontainers]
---
# Test Containers
애플리케이션의 기능을 테스트할 때 데이터베이스나 다른 여러 컴포넌트와 엮인 기능의 경우 JUnit등의 테스트 프레임워크를 사용해서 테스트를 하기가 곤란한 경우가 많다. 

어떠한 테스트를 수행할 때 상황에 따라 성공여부가 나뉜다면 전체 애플리케이션 테스트가 매우 번거로울 수 있다. 예를들어 내 컴퓨터에서 테스트를 했을 때 모두 Pass했지만 다르 항상 일정한 결과물을 이러한 프레임워크를 사용한 테스트의 경우 대게는 어떠한 경우에는 실패

# refer to
https://www.testcontainers.org/

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwNjk2NDY3ODEsMTMyMzUwNDQzMV19
-->