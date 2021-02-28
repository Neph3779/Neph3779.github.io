---
layout: post 
title: 09. Handling JavaScript Alerts using Selenium
image:
categories: Selenium
tags:
  - Selenium
  - Java
---

Handling JavaScript Alerts using Selenium

Alerts는 html태그가 아니라, Javascript로 인하여 동작한다. 그렇기 때문에 어떠한 요소에 대한 정보를 얻을 수 없다.

이럴 때는 Driver를 switching하여 대응 할 수 있다.
- - - -
```java
driver.switchTo().alert();
```

를 사용하면 인터넷 창 상의 모든 alert에 대응할 수 있게된다.

이후 `accept()` 나 `dismiss()` 를 실행해주면 된다.

- - - -
Confirm alert에는 Yes나 Cancel가 있는데, cancel을 하고자하면 `dismiss()` 를 사용하면 될 것 같다.

```java
/* Confirm or OK Button */      
				driver.findElement(By.xpath("//*[@id=\"alertbtn\"]")).click();

        Thread.sleep(3000);
        System.out.println(driver.switchTo().alert().getText());
        driver.switchTo().alert().accept();
        Thread.sleep(3000);
/* dismiss */
        driver.findElement(By.xpath("//*[@id=\"confirmbtn\"]")).click();
        Thread.sleep(2000);
        driver.switchTo().alert().dismiss();
```

