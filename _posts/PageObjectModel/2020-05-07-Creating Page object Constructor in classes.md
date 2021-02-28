---
layout: post
title: Creating Page object Constructor in classes
image:
categories: Page-Object-Pattern
tags:
  - Page Object Pattern
  - Java
---

예컨데, homepage와 login page를 테스트한다고 하면,

Homepage 라는 java 파일 생성
Homepage 오브젝트를 정의.
loginPage 라는 java파일 생성.
loginPage 오브젝트를 정의.

-> 페이지별로 파일을 생성해주어야함.

- - - -
이후 Testcase를 생성.
로그인이라는 케이스를 한다면, 홈페이지 클래스 에서 로그인하는 메소드를 구현해놓아야함.

- - - -
driver의 경우, 테스트클래스에서 오브젝트를 생성해준 다음, 페이지 클래스로 parameter의 형식으로 넘겨줌.

```java
@Test
public void Login() {

    System.setProperty("webdriver.chrome.driver", ""/Users/yoodahun/Documents/Github/Java/Selenium WebDriver with Java/chromedriver");
    WebDriver driver = new ChromeDriver();
    driver.get(“https://mail.rediff.com/cgi-bin/login.cgi”);

    LoginPage loginPage = new LoginPage(driver);

    loginPage.emailId().sendKeys("Hello");
    loginPage.password().sendKeys("password");
    loginPage.submit().click();

}

```





