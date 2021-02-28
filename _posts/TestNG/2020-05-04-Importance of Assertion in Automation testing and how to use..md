---
layout: post
title: Importance of Assertion in Automation testing and how to use.
image:
categories: TestNG
tags:
  - TestNG
  - Java
---

TestNG is on of the testing framework.
Assertion is to validate the result whatever selenium return is expect is true or false?

- - - -
TestNG.jar를 MVN Repository 혹은 Import package / module하여 등록한다.

Maven 으로 관리한다면, 아래와 같다.

```xml
<!-- https://mvnrepository.com/artifact/org.testng/testng -->
<dependency>
    <groupId>org.testng</groupId>
    <artifactId>testng</artifactId>
    <version>7.0.0</version>
    <scope>test</scope>
</dependency>
```

(어떠한 에러때문에 7.0.0 버전을 사용하는 중... 무슨 에러인지는 까먹었음).

