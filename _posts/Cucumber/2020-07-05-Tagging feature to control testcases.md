---
layout: post
title: Tagging feature to control testcases
image:
categories: Cucumber
tags:
  - Cucumber
  - Java
---



테스트케이스를 테스트종류에 따라 나누고자할때 유용한 방법은 무엇일까?
전체 테스트케이스를 실행하지 않고도 필요한대로 정의한 테스트케이스만 실행하는 것.

```gherkin
@SmokeTest
Scenario: Homepage default login
  Given User is on Netbanking landing page
  When User login into application with "Jin" and "1234"
  Then Homepage is populated
  And Cards Displayed are "false"


```

테스트케이스 시나리오명 앞에 임의의 `Tag` 를 생성해준다. 띄어쓰기는 인식하지 않는다.

Cucumber는 정해진 태그가 아닌, custom tag를 사용할 수 있다. 또한 여러개의 태그를 지정해줄 수도 있다.

tag를 지어주었으면 옵션을 설정해주어야 한다.

예를 들어 `SmokeTest` 태그가 달린 테스트만 하고싶다면?
```java
@CucumberOptions(
        plugin = {“pretty”, “html: target/reports/cucumber-html-report”},
        features = “src/test/java/features”,
        glue = "stepDefinitions",
        tags = "@SmokeTest"

)
public class TestRunner {
}
 
```
TestRunner 파일에 옵션으로 `tags`  를 지정을 해주면 된다.

## 여러가지의 태그를 지정해서 동시에 하고 싶다면?
```java
tags = “@RegressionTest or @SmokeTest”
```
OR 나 AND로 감싸줄 수 있다.



