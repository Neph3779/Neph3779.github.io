---
layout: post
title: 15. Links
image:
categories: Selenium
tags:
  - Selenium
  - Java
---



## How to links count?

```java
List<WebElement> links = driver.findElements(By.tagName("a"));
```

모든 링크는 a태그를 사용하기 때문에, a태그로 확인해줄 수 있음. 위 코드의 경우 어떤 페이지 안의 모든 link를 얻게됨.



---

## How to open the links in separate tabs.

Window는 Control / MacOS는 Command키를 누르고 링크를 클릭하면, 새로운 탭으로 열리게 된다.

하나하나 뒤로가기 버튼을 누르지 않아도 확인할 수 있음.

```java
String clickOnLinks = Keys.chord(Keys.CONTROL, Keys.ENTER);
for(int I = 1; I < firstColumnLinks.size(); I++) {
    firstColumnLinks.get(i).sendKeys(clickOnLinks);
}

```

어떠한 키를 `chord()`를 이용해 같이 누르는 것. Keyboard 액션으로 새창을 열 수 있다.

위 방법을 이용하면 여러개의 링크를 동시에 열 수 있다.

