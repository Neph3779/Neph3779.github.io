---
layout: post
title: Parameterizing Selenium tests with Cucumber
image:
categories: Cucumber
tags:
  - Cucumber
  - Java
---

## Two ways of Data-driven Test with Cucumber

Cucumber에는 `.feature` 파일에서 데이터를 직접 입력하여 테스트를 진행하는 2가지 방법이 있다.



### dataTable for Data-driving tests

똑같은 스토리를 여러 번 계속 반복해서 쓴다면 시나리오의 양이 엄청나게 늘어날 것 임.
매우 비효율적인 것.

```gherkin
Scenario: Homepage default login
  Given User is on Netbanking landing page
  When User sign up with following details
    |jenney|abcd|john@abcd.com|Austraila|3242302
    Then Homepage is populated
  And Cards Displayed are "false"

```

`when` 에 조건을 넣어주고 여러가지 데이터옵션을 넣어준다.
데이터는 파이프로 구분할 수 있다.
위와 같은 방법으로 data-driving test를 구현해볼 수 있다.

만일 다른 프레임워크로 한다면 보통은 excel로 구현해야하는데, excel파일을 읽어들이고 원하는 셀을 찾은 다음, 해당 셀의 데이터를 읽어들이는 것은 생각보다 간단한 일은 아닐 수도 있다.

Step definition은 어떻게 정의할 수 있을까?

```java
@When(“User sign up with following details”)
public void userSignUpWithFollowingDetails(DataTable table) {
    List<List<String>> obj = table.asLists();
    System.out.println(obj.get(0).get(0));

}

```

`DataTable` 이라는 타입으로 정의할 수 있다.
이 table변수에서, `asLists()` 로 꺼내오면, String이 담긴 리스트의 리스트가 나온다.

즉,

```
1 row : a, b, c, d
2 row : e, f, g,h
.
.
.
```

와 같은 것이 나온다.
이후 `println`을 보면 알겠지만, 0번을 을 지정하게되면 첫번째줄. 여기서 또 0번을 하면 첫번재 컬럼을 지정하는 것이다.

---

### Parameterizing Selenium tests with Cucumber

Cucumber 시나리오를 데이터만 다르게 하여 똑같은 시나리오를 반복하여 실행하게 하려면...?

`Scenario outline` 을 선언하고 `Example` 키워드를 사용해야한다.



### Scenario outline

어떠한 valiable값을 parameter로 넘겨, 값은 같지만 시나리오의 결과는 같을 때,  `Scenario outline`을 사용한다.

### Example

어떠한 테이블을 작성해주는데, 컬럼간의 구분은 파이프로 구분함.

```gherkin
Feature: Login into Application
  Scenario Outline: Positive test validating login
    Given Initialize the browser with chrome
    And Navigate to Site
    And Click on Login link in homepage to land on sign-on in Page
    When User enters <username> and <password> and logs in
    Then Verify that user is not logged in

Examples:
|username |password|
|”test99@gmail.com” |”123456”|
|”test123@gmail.com” |”123456”|



```

이 때, `Scenario outline` 내부에 파라미터로 넘겨줄 부분은 `<>` 로 감싸주어야 한다.

이 때, parameter를 입력받는 시나리오 라인의 구현은,
Annotation 에는 paramter 표현으로 선언하되, 메소드 이름은 시나리오 라인 그대로 작성해야 인식한다.

```java
@When(“User enters {string} and {string} and logs in”)
public void userEntersUsernameAndPasswordAndLogsIn(String username, String password) {
    loginP = new LoginPage(driver);
    loginP.getEmailInput().sendKeys(username);
    loginP.getPasswordInput().sendKeys(password);
    loginP.getLoginButton().click();

}

```



[Cucumber - Scenario Outline - Tutorialspoint](https://www.tutorialspoint.com/cucumber/cucumber_scenario_outline.htm)

