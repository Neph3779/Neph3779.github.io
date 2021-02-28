---
layout: post 
title: 08. Validating if UI Elements are disable or enabled with Attributes
image:
categories: Selenium
tags:
  - Selenium
  - Java
---

Validating if UI Elements are disable or enabled with Attributes



isEnable() 메소드가 있긴하나 웹사이트의 쓰인 기술에 따라 적용되지 않을 때도 있었음.

➡ disable상태라도 클릭을하게되면 활성화되기 때문

어떠한 요소의 색상이 바뀌는 것을 캐치하면 어떨까?

- - - -
Style 속성의 opacity값을 체크하여 테스팅.
returnDate.getAttribute(“style”).contains(“”)

```java
if(returnDate.getAttribute(“style”).contains(“1”)) {
    System.out.println(“enable!”);
    Assert.assertTrue(true);
} else {
    System.out.println(“disable!”);
    Assert.assertTrue(false);
}
```

