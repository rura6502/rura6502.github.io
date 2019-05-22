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

## Create Jenkinsfile

젠킨스가 젠킨스 파일을 생성하는 방법은 3가지가 있음

### Blue Ocean

개인적으로 가장 강력 추천. 기본으로는 설치되어 있지 않고 추가로 설치해야되는 플러그인인데 플러그인 매니져에서 검색하면 바로 나옴. 약간 아재냄새나는 젠킨스와는 달리 수려한 UI를 가지고 있으며 UI를 통해서 파이프라인의 각 단계를 설정하고 코드를 삽입할 수 있음.

### Classic UI

이름에서도 알수있다시피 기본 젠킨스 UI의 New Item 항목에서 제공하는 파이프라인을 만듬. ```Blue Ocean```보다 예쁘진 않지만 기능에 충실하고 오히려 세세한 부분을 적기엔 블루오션보다 더 편한 감이 있음

### SCM

```Classic UI```에서 제공하는 텍스트 에디터로 처음부터 끝까지 코드로 짤 수 있음. 파이프라인을 코딩하는데 실력이 충분하지 않다면 블루오션이나 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTgxNzkzNTMyNiw3NjU5Nzg2NjJdfQ==
-->