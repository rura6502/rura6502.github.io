---
layout: post
title:  Jenkins & Pipeline 개념
date:  2019-05-21 00:00:00 +0900
categories: [Jenkins, Pipeline, CD/CI]
---

공식문서에서는 Suite of plugins that supports implementing and integrating continouous delivery pipelines 라고 되어있음. 쉽게말해서 자동으로 빌드, 배포등을 할 수 있도록 관련된 플러그인들을 모아서 관? 처럼 만들어줌.

젠킨스가 제공하고 또는 설치할 수 있는 플러그인들은 매우 다양한데 예를들어 깃, 도커 빌드, 쉘스크립트 등 매우 많음. 이를 배열해서 ```동작1->동작2->동작3``` 이런식으로 배열하는 것을 파이프라인이라고 함.

> 예제는 일렬로 되어있지만 병렬로도 동작이 가능함

기본적인 파이프라인의 순서는 어떤 제공자(깃 서버 등)으로 부터 결과물(소스)를 받아서 어떠한 동작(junit 테스트, maven/gradle 빌드, c 컴파일 뿐만 아니라 복사 등 다양한 행동)을 하고 이를 배포(개발/운영 서버로 전송 후 스크립트 실행 또는 레포트 이메일 전송) 하는 순서로 파이프라인이 진행 됨. 물론 이는 사용자가 배치를 하기 나름임.

> 깃 서버로부터 checkout -> test -> compile > build > deploy

젠킨스에서는 이 파이프라인을 Pipeline DSL이라는 Groovy로 이루어진 코드로 작성하여 정의하고 관리함.

> 여기서 문서상으로 limited form Groovy라고 설명하고 있으므로 프로그래밍 언어로써의 Groovy의 모든 문법을 지원하지는 않을 것으로 예상 됨.

파이프라인에는 Declarative/Scripted Pipeline 두가지 종류(후술)가 있음.

파이프라인을 만들기 위해선 몇가지 방법을 제공함

* ```Blue Ocean``` : Blue Ocean UI라는 아주 예쁜 UI를 제공하는데 이를 사용해서 ```Jenkinsfile```을 만들고 소스 프로젝트에 커밋할 수 있음.
* ```Classic UI``` : Jenkins에서 원래 제공하는 UI. Blue Ocean UI에 비해서 매우 많이 투박함.
* ```SCM``` : 직접 ```Jenkinsfile```을 제작

## Concent

파이프라인을 구성하기 위한 기본 지식

### Pipeline
빌드 프로세스를 기술한 전체 CD 모델. 애플리케이션을 빌딩하고 테스팅하고 배포하기위한 stage 들로 구성되어 있음. Pipeline을 구성하는 요소를 block이라고 부르고 이 block에는 다양한 종류들이 있음.

### Node
Pipeline을 실행하는 주체, machine이라고 표현함. 

### Stage - subset of tasks(steps)
파이프라인에서 수행되는 스텝(step)의 집합. 여러개의 스텝들이 모여서 build stage, test stage, deploy stage를 구성함.

### Step - single task
파이프라인을 구성하는 가장 작은 단위. 실제 파이프라인이 수행해야 하는 단위 명령어들이 들어있다고 보면 됨. 예를들어 mkdir, cp, sh, make 등.

## Pipeline Syntax(Declarative/Scripted Pipeline Syntax)

파이프라인 문법은 선언적 파이프라인 문법(Declarative) 과 스크립티드 파이프라인 문법(Scripted)으로 나뉨.

### Declarative Pipeline

```pipeline``` block으로 시작함.

```groovy
pipeline {
 agent any
 stages {
  stage('Build') {
   steps {
    // do Build tasks
   }
  stage('Test') {
   steps {
    // do Test tasks
   }
  }
  stage('Deploy') {
   steps {
    // do Deploy tasks
   }
  }
}
```

### Scripted Pipeline

하나 또는 그 이상의 node block으로 구성되어 있음. 

```groovy
node {
 stage('Build') {
  // Scripted Pipeline syntax에서 stage블록은 있어도되고 없어도됨.
  // 하지만 가독성을 위해서 서술. Jenkins UI 에서도 잘 볼 수 있음.
 }
 stage('Test') {
  //
 }
 stage('Deploy') {
  //
 }
}
```

### Pipeline Example
```groovy
// Declarative Pipeline
pipeline {
 agent any // jenkins에게 이 파이프라인을 실행할 executor를 할당하라고 지시
 options {
  skipStagesAfterUnstable()
 }
 stages { // 각 스테이지 단계에서 수행해야될 스텝들을 명시
  stage('Build') {
   steps {
    sh 'make'
   }
  }
  stage('Test') {
   steps {
    sh 'make check'
    junit 'reports/**/*.xml'
   }
  }
  stage('Deploy') {
   steps {
    sh 'make publish'
   }
  }
 }
}
```

```groovy
// Scripted Pipeline
node {
 stage('Build') {
  sh 'make'
 }
 stage('Test') {
  sh 'make check'
  junit 'reports/**/*.xml'
 }
 if (currentBuild.currentResult == 'SUCCESS') {
  stage('Deploy') {
   sh 'make publish'
  }
 }
}
```



## refer to
[Pipeline](https://jenkins.io/doc/book/pipeline/)
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTY2OTYwOTg1N119
-->