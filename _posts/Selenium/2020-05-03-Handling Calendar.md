---
layout: post
title: 16. Handling Calendar
image:
categories: Selenium
tags:
  - Selenium
  - Java
---



## How to select specific date or month in Calendar

calendar의 element들은 대부분 **같은 클래스**를 가지고 있음.
거기서 `equal() `을 사용하여 얻고자하는 날짜를 가진 요소를 선택하는것.

```java
List<WebElement> elements = driver.findElements(By.className(“day”));

for (WebElement element : elements) {
    if(element.getText().equals("31")) {
        element.click();
        break;
    }
}

```


- - - -

## Generic method to handling month and date
month에 해당하는 부분의 텍스트정보를 얻고, 해당 텍스트 정보가 나올때까지 캘린더를 이동시키는 방식.
```java
// June 24, 2020 select month and date
WebElement month = driver.findElement(By.xpath(“/html/body/div[4]/div[1]/table/thead/tr[1]/th[2]"));
while(!month.getText().contains("June")) {
    Thread.sleep(1000);
    driver.findElement(By.xpath("/html/body/div[4]/div[1]/table/thead/tr[1]/th[3]")).click();
}

List<WebElement> elements = driver.findElements(By.className("day"));

for (WebElement element : elements) {
    if(element.getText().equals("24")) {
        element.click();
        break;
    }
}

```

위 코드 첫 번째 `while` 에서, June 문자열을 찾지못하면 다음 달을 선택하도록 되어있음.

이후 찾는다면, 그 다음에는 날짜를 찾게된다.



---

날짜 계산방법은, Java의 Date class를 이용하는 건데,

아래의 링크 글에 설명이 잘 되어있다.

https://coronasdk.tistory.com/766

