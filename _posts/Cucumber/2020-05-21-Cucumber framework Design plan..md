---
layout: post
title: Cucumber framework Design Plan and BDD
image:
categories: Cucumber
tags:
  - Cucumber
  - Java
---



	- Understand the Importance of Behavior Driven Development
	- Cucumber Framework Architecture and its core functionalities.
	- Selenium integration with cucumber.

Cucumber를 시작하기 전에, BDD에 대해서 알아야 할 필요가 있다. Cucumber는 BDD와 매우 밀접한 framework이다. 
그 다음 핵심 기능등을 배우고, 자동화의 중심인 selenium과의 조합을 확인해야 한다.

---



## Template in writing Business requirements

Testcases specifications in Behavior Driven Development

비즈니스 관계자, Owner들의 요구사항은, 즉 문서화되어 개발자와 테스터들에게 공유된다. 

BDD는 Agile 개발방법 중 하나. 비즈니스 요구사항은 즉 테스트케이스가 되며, 이 테스트 케이스는 개발자들에게 하나의 참조사항이 되기도 한다. 요구사항은 개발자와 테스터 양쪽에 모두 참고가 된다.

요구사항은 테스터와 개발자, 비즈니스 관계자 모두 이해가 다를 수 있다. 인식하는 지식이나 방법이 다르기 때문. 이것들을 모두 공통적인 레벨로 통일할 필요가 있다.

비즈니스 관계자들은 어떠한 특정한 템플릿으로 요구사항을 정의해야할 필요가 있다.

Testcases should be defined in a Business level by following a ubiquitous language
Ubiquitous language is a formal language that is shared by all members of a software development team - both software developers and no-technical personnel.

즉, 요구사항을 특정한 템플릿으로 서술하여 개발자와 테스터가 모두 같은 레벨에서 인식할 수 있도록 하는 것.




