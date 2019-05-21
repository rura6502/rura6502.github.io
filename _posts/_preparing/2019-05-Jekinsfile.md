---
layout: post
title:  Jenkinsfile and Declaritive/Srcipted Pipeline
date:  2019-05-21 00:00:00 +0900
categories: [Jenkins, Pipeline, CD/CI, Jenkinsfile]
---

젠킨스는 파이프라인을 어떻게 구동할지, 이 파이프라인이 어떤 일을 해야 하는지 ```Jenkinsfile```에 정이된 행동을 수행하는데, 자신이 내려받은 소스에서 파일이름이 '```Jenkinsfile```'(이하 젠킨스파일) 인 파일을 읽어서 그 정보를 획득한다.

젠킨스 파일은 Groovy 언어로 되어있고 그 양식은 Declarative, Scripted로 나뉜다. 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2NDY5NTY2ODhdfQ==
-->