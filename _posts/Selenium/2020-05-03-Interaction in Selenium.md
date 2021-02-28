---
layout: post
title: 12. Interaction in Selenium
image:
categories: Selenium
tags:
  - Selenium
  - Java
---

어떠한 상호작용들. 대부분 Javascript에 의한 것들.

Action class를 사용하여 할 수 있다. Action object를 실행하기 위해서는 `build()`하고 `perform()`을 실행해주어야 함.



## Mouse over

```java
Actions action = new Actions(driver);
//if there is no build() and perform(), doesn't execute.
action.moveToElement(driver.findElement(By.xpath("//*[@id=\"nav-link-accountList\"]"))).build().perform();

```

**Action에는 반드시 driver를 변수로 넘겨줘야함.**



---

## Input the Capital letters.

```java
//Input capital letter
action.moveToElement(driver.findElement(By.xpath("//*[@id=\"twotabsearchtextbox\"]"))).click().build().perform();
action.keyDown(Keys.SHIFT).sendKeys("hello").build().perform();


```

Click을 하여 textbox를 활성화 한 다음,

Shift를 클릭한 채로 원하는 단어를 입력하게 하면 됨.



---

## Right click

```java
//Right click.
        action.moveToElement(driver.findElement(By.xpath("//*[@id=\"nav-link-accountList\"]"))).build().perform();
        action.contextClick().build().perform();

```

`contextClick()` 을 이용하면 오른쪽 클릭이 가능함.



---

## Select Entire Text sentence.

```java
action.doubleClick().build().perform();
```



모든 텍스트를 전체 더블클릭하면 선택할 수 있다.