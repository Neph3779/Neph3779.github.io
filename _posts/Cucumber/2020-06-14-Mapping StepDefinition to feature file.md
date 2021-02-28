---
layout: post
title: Mapping StepDefinition to feature file
image:
categories: Cucumber
tags:
  - Cucumber
  - Java
---



Step Definition file을 만들어서 각 스텝을 정의해줄 필요가 있음.

Test > java 밑에 stepDefinition이라는 패키지를 만들어서 관리해주면 좋다.

```java
@Given(“^User is on NetBanking landing page$”)
public void user_is_on_netbanking_landing() {

}
```
### 키워드에 대한 정의.

메소드 이름은 보통 시나리오의 해당 키워드와 거의 비슷한 형식으로 작성하면 좋다.
Annotation의 내용은 `^` 로 시작하여 `$` 로 마감해주어야한다.

각 step에 대한 정의를 작성해준다. 해당 정의 annotation 에 매칭되는 메소드를 작성하고, 내부에 코드를 구현해준다.

StepDefinition file matches with TagName and description
- - - -

```java
public class stepDefinition {

    @Given(“^User is on NetBanking landing page$”)
    public void user_is_on_netbanking_landing() {

    }


    @When("^User login into application with username and passwod$")
    public void userLoginIntoApplicationWithUsernameAndPasswod() {
        
    }

    @Then(“^Homepage is populated$”)
    public void homepageIsPopulated() {
        
    }

    @And(“^Cards are displayed$”)
    public void cardsAreDisplayed() {
    }
}

```

`Feature` 파일과 `Annotation` 의 내용과 매칭됨.


