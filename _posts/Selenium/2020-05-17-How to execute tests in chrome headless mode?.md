---
layout: post
title: 24. How to execute tests in chrome headless mode?
image:
categories: Selenium
tags:
  - Selenium
  - Java
---

실제로 웹브라우저를 실행하지 않고 background에서만 동작하게 하는 것.
**렌더링**을 하지 않음.

실제로 비교하여 빠르거나하진 않으나, Jenkins에서 할 때는 대부분 headless를 선호하는 경향이 있음.

```java
ChromeOptions chromeOptions = new ChromeOptions();
driver = new ChromeDriver(chromeOptions.setHeadless(true));
```

driver object를 생성할 때, `chromeOptions` 을 parameter로 넘겨준다.



---

headless로 하다가는 로봇인지 아닌지 체크를 묻고는 하는데, 그때에는 properties를 추가해줘야할 필요가 있다.
**user-agent=mrbean**

```java
ChromeOptions chromeOptions = new ChromeOptions();
chromeOptions.addArguments(“headless”,”user-agent=mrbean”);
```

