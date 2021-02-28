---
layout: post
title: 16. Driver Scope
image:
categories: Selenium
tags:
  - Selenium
  - Java
---



## Driver scope

어떠한 범위만 확인하는 부분적인 driver를 사용하여, driver의 범위를 지정해줄 수가 있다.

```java
//local driver
    WebElement footerDriver = driver.findElement(By.id("gf-BIG"));
    List<WebElement> footerLinks = footerDriver.findElements(By.tagName("a"));

//find links only first columns
    WebElement columnsDriver = footerDriver.findElement(By.xpath("//table/tbody/tr/td[1]/ul"));
    List<WebElement> firstColumnLinks = columnsDriver.findElements(By.tagName("a"));
    System.out.println(firstColumnLinks.size());


```

어떠한 특정 요소만 확인하는 WebElement요소를 생성하고 **해당 요소를 driver처럼 사용하는 것**.

First columns에 있는 링크를 확인하기 위해서는, 이미 `footerDriver` 에 지정한 요소가 있으므로, 해당 요소를 기준으로하여 다시 작은 요소를 찾아 들어가면 됨.

*Relative적인 접근 방법으로 이해한다면...*?

