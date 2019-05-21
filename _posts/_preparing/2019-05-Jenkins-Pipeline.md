---
layout: post
title:  Jenkins Pipeline
date:  2019-05-11 00:00:00 +0900
categories: [, ]
---

## Concent

### Pipeline
빌드 프로세스를 기술한 전체 CD 모델. 애플리케이션을 빌딩하고 테스팅하고 배포하기위한 stage 들로 구성되어 있음. Pipeline을 구성하는 요소를 block이라고 부르고 이 block에는 다양한 종류들이 있음.

### Node
Pipeline을 실행하는 주체, machine이라고 표현함. 

### Stage - subset of tasks(steps)
파이프라인에서 수행되는 스텝(step)의 집합. 여러개의 스텝들이 모여서 build stage, test stage, deploy stage를 구성함.

### Step - single task
파이프라인을 구성하는 가장 작은 단위. 실제 파이프라인이 수행해야 하는 단위 명령어들이 들어있다고 보면 됨. 예를들어 mkdir, cp, sh, make 등.

## Pipeline Syntax

파이프라인 문법은 선언적 파이프라인 문법(Declarative) 과 스크립티드 파이프라인 문법(Scripted)으로 나뉨.

### Declarative Pipeline

<!--stackedit_data:
eyJoaXN0b3J5IjpbODIxMjIzMDQ3LDE0Nzk4NTc3MDldfQ==
-->