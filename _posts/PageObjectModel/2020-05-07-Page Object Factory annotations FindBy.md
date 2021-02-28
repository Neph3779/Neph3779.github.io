---
layout: post
title: Page Object Factory annotations FindBy
image:
categories: Page-Object-Pattern
tags:
  - Page Object Pattern
  - Java
---

Page Object Class를, Selenium의 `Factory` 라는 개념으로 새롭게 정의할 수 있다. 이중 `@FindBy` Annotation을 써서 구현해볼 수 있다.



- - - -

## @FindBy
```java
// By username = By.xpath("//*[@id=\'login1\']");
@FindBy(xpath = "//*[@id=\\\'login1\\\']")
WebElement username;

```
위와 아래는 똑같은 행위를 함.

`FindBy` annotation은 WebElement 요소를 지정된 parameter로 찾게됨.


## Constructor
```java
public LoginPageFactory(WebDriver driver){
    this.driver = driver;
    PageFactory.initElements(driver,this);
}

```



https://www.selenium.dev/selenium/docs/api/java/org/openqa/selenium/support/package-summary.html

https://intellipaat.com/community/33201/how-how-id-in-selenium

https://www.softwaretestinghelp.com/page-object-model-pom-with-pagefactory/


