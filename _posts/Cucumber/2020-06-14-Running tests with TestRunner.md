---
layout: post
title: Running tests with TestRunner
image:
categories: Cucumber
tags:
  - Cucumber
  - Java
---

Test를 실행하기 위해서, feature file과 runner파일을 묶어줄 필요가 있다.

Junit이라면 `@Runwith(Cucumber.class)` 과  `@CucumberOption(feature="{featureFilePath}", glue="{stepDefinition}"`
TestNG라면 `@CucumberOption(feature="{featureFilePath}", glue="{stepDefinition}"` 만 선언해주면 된다.

---

#### Features
#### features

` @CucumberOptions(features = “src/test/java/features” , glue = “stepDefinition”  )`
에서, `features` 옵션에서 디렉토리까지만 지정해주면, 해당 패키지 내부의 모든 feature 파일을 실행시킬 수 있다. 반대로 특정 feature파일만 실행시키고자 하면, feature파일의 `이름.feature` 까지 지정해주면 된다.

`src/test/java/features/Login.feature`

#### glue
Step definition파일을 지정하는 파라미터. 패스 지정은 필요없으며, stepDefinition은 TestRunner의 패키지와 같은 레벨. 같은 부모 패키지를 가지고 있어야 한다.

#### plugin

Cucumber 실행에 필요한 외부 플러그인을 선언.

#### Tags

실행시키고 싶은 특정 태그들을 지정.

```java
@RunWith(Cucumber.class)
@CucumberOptions(
        plugin = {“pretty”, “HTML: target/cucumber”},
        features = "src/test/java/features",
        glue = "stepDefinitions"

)
```

- - - -



## AbstractTestNGCucumberTests

TestNG에서 Cucumber를 이용한 테스트를 실행하고자 할 때에는, TestRunner에 `AbstractTestNGCucumberTest` 를 상속시켜줘야한다.

```java
@CucumberOptions(features = “src/test/java/features”,
                 glue = “stepDefinition” )
public class TestRunner extends AbstractTestNGCucumberTests {
}

```




