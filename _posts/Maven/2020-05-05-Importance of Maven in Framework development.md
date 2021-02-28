---
layout: post
title: Importance of Maven in Framework development
image:
categories: Maven
tags:
  - Maven
  - Java
---

Apache *Maven* is a software project management and build management tool for Java frameworks.

### Why?

- Central repository to get dependencies
  		- 필요한 의존성을 해결하기 위한 중앙 레포지토리가 준비되어있다.
  - [Maven Repository: Search/Browse/Explore](https://mvnrepository.com/)
    	- 여러 사람들이 같은 프로젝트를 할 때, 각각의 사람들은 jar파일을 다운받아야하는데 maven을 사용하면 자동적으로 그것을 다운받아줌.
- Maintaining common structure across the organizing
	- 어떠한 기초가 되는 뼈대구조를 미리 만들 수 있게됨.
	- src와 test 폴더의 기본 구성. Resources의 지정.
- Flexibility in integrating with CI tools
  - Jenkins와의 연계가 좋음.
- Plugins for test framework execution.
  - 프레임워크의 실행을 위한 기타 플러그인 설치가 용이.


