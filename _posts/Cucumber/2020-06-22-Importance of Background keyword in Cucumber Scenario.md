---
layout: post
title: Importance of Background keyword in Cucumber Scenario
image:
categories: Cucumber
tags:
  - Cucumber
  - Java
---

## Background
`Background` 키워드는, 어떠한 기본이 되는 시나리오 혹은 절차들을 하나로 정리한 것이다. 모든 시나리오의  전제로 되어야하는 내용들

```gherkin
Background:
  Given Validate the browser
  When Browser is triggered
  Then Check if browser is started

```
위와 같이, `Background` 키워드를 작성하고, 내부는 일반 시나리오처럼 작성해준다.

Background keyword의 내용들도 stepDefinition을 작성해주어야할 필요가 있음.

```java
@Given(“Validate the browser”)
public void validateTheBrowser() {
    System.out.println(“validate the browser”);

}

@When("Browser is triggered")
public void browserIsTriggered() {
    System.out.println("browser is triggered");

}

@Then("Check if browser is started”)
public void checkIfBrowserIsStarted() {
    System.out.println(“Browser is Displayed”);
    System.out.println("Background keyword done ----------------");
}

```

작성한 background keyword들의 메소드들은, 해당 background가 선언된 feature file내부의 각각의 Scenario들이 실행되기 바로 직전에 실행된다.
**Scenario마다 한 번씩 실행된다.**

모든 시나리오가 동일한 전제조건을, 배경을 가지고 있다는 것을 전제로 하고 실행시키는 것이 `background`임.

즉, background가 선언되지 않은 다른 feature파일에서는 실행되지 않음.

---

또한 `Background` keyword는 feature 파일의 최상단에서 선언되어야 함.

다만, feature file 내부에, 전제조건이 다른 케이스가 있다면...?? 못쓴다고 봐도 무방.
