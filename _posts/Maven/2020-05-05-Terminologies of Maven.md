---
layout: post
title: Terminologies of Maven
image:
categories: Maven
tags:
  - Maven
  - Java
---

용어 정리.

### GroupId 
Maven repository에 올려져있는 jar들에는 group-id가 Unique하게 부여되어있음.
Project id

### artifactID 
Group-id의 sub-id. Jar name.

### version
해당 jar파일의 version.



- - - -
### Command 로 maven을 생성할 시.

`maven archetype:generate -DgroupId=~~~ -DartifactId-projectname -DarchetypeArtifactId=maven-archetype-quickstarts -DinteractiveMode=false `


- gereate:생성
- groupId:URL의 거꾸로. 패키지 네임.
- artifactID:project name
- archetypeArtifactID=maven-archetype-quickstarts  : 기본 프로토타입.





