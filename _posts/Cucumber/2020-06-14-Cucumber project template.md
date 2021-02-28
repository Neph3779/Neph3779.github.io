---
layout: post
title: Cucumber project template
image:
categories: Cucumber
tags:
  - Cucumber
  - Java
---

Cucumber는 `Gherkin` 이라는 단어? 가 필요한데,
 intellij의 경우에는 미리 설치되어 있음.

만약 eclipse를 사용한다면, marketplace에서 `Naturist` 를 다운받아야함.
cucumber로 검색하면 나올지도…?

- - - -
Cucumber는 그냥 연습하고싶어도 main 메소드로는 실행할 수 없음.
기본 skeleton 파일이 필요함.

maven은 skeleton 파일을 제공해줌. maven으로 설치하게 되면 자동적으로 구성을 해줄 것임.

cucumber는 quick start template을 maven을 통해 제공함.
maven자체가 QuickStart 옵션을 제공함.
architype에서 quickstart를 선택하여 maven을 생성함.

- - - -
`pom.xml` 에 필요한 라이브러리를 선언해줌.
IntelliJ 의 경우 `cucumber-java` 가 기본으로 들어있지만, eclipse의 경우는 그렇지 않기 때문에 같이 선언해주어야함.

```xml
<dependency>
  <groupId>org.testng</groupId>
  <artifactId>testng</artifactId>
  <version>7.0.0</version>
  <scope>test</scope>
</dependency>

<dependency>
  <groupId>io.cucumber</groupId>
  <artifactId>cucumber-testng</artifactId>
  <version>5.5.0</version>
</dependency>

```

내 경우는 testng와 같이 조합할 예정이므로, junit을 지워주고 testng를 선언해주었다.
