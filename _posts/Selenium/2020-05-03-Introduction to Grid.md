---
layout: post
title: 22. Introduction to Selenium Grid
image:
categories: Selenium
tags:
  - Selenium
  - Java
---



## What is Selenium grid?
Running test **different machine and different browser**.
Selenium-grid support distributed test execution and Parallel test. In multiple machines on Selenium grid.

Selenium gird는 여러 기기와 여러 브라우저들을 동시에 테스팅할 수 있게 도와준다. 병렬적인 테스팅이 가능하다.

### Hub
Central point that will receive all the test request and distribute them. node.
Machine containing the hub is where the tests will be triggered. But browser being automated on the node.

hub에서 각각의 node들로 테스트 실행등을 뿌려주는 역할.

### Node
Node is Selenium instance.
노드에는 필요한 java 혹은 jar,  chrome driver가 설치되어있어야한다.




