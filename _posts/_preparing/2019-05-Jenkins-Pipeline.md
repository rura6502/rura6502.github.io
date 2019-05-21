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
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0MTk3MDUwMTcsLTQwOTY5NDUzOSwxND
c5ODU3NzA5XX0=
-->