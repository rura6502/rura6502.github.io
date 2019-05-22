---
layout: post
title:  Jenkinsfile and Declaritive/Srcipted Pipeline
date:  2019-05-21 00:00:00 +0900
categories: [Jenkins, Pipeline, CD/CI, Jenkinsfile]
---

젠킨스는 파이프라인을 어떻게 구동할지, 이 파이프라인이 어떤 일을 해야 하는지 ```Jenkinsfile```에 정의된 행동을 수행하는데, 자신이 내려받은 소스에서 파일이름이 '```Jenkinsfile```'(이하 젠킨스파일) 인 파일을 읽어서 그 정보를 획득함.

도큐면트에서는 이렇게 함으로써 파이프라인 코드를 review/iteraction 할 수 있고 소스들과 함께 버전컨트롤을 함으로써 팀원들과 배포와 관련된 내용을 공유하고 감시할 수 있다고 말하고 있음. 즉 젠킨스파일에는 배포하려는 서버의 계정/비밀번호나 어떠한 보안정보도 들어있으면 안됨.

> ssh 등을 사용할 때 계정정보등을 넣으면 안됨

젠킨스파일은 Groovy 언어로 되어있고 그 양식은 Declarative(버전 2.5 이상), Scripted로 나뉨.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2NDE4NzAxMDcsNzY1OTc4NjYyXX0=
-->