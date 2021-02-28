---
layout: post
title: 14. Frames and Drag & Drop
image:
categories: Selenium
tags:
  - Selenium
  - Java
---



## Frames

Frames is component. Container. **Separately**
내용을 독립적으로 보여주는 컨테이너와 같은 존재. 한 페이지에서 다른 페이지를 새롭게 표시하고자 할 때.

Selenium은 frame을 **핸들링할 수 없다.**
명시적으로 제어를 해주어야한다.

`switchTo().frame()` 프레임을 인식할 수 있게 해준다.

frame()은 3가지 종류가 존재하는데, 인덱스나 문자열 혹은 webElement로 지정해줄 수 있음.

인덱스로 지정해줄 때는, 페이지에 frame이 몇개나 있는지 확인을 해주어야하는데, 이때는 findElements(By.tagName(“”)).size()로 확인해줄 수 있음.

```java
				 driver.switchTo().frame(driver.findElement(By.xpath("//*[@id=\"content\"]/iframe")));

        WebElement dragBox, dropBox;
        dragBox = driver.findElement(By.xpath("//*[@id=\"draggable\"]"));
        dropBox = driver.findElement(By.xpath("//*[@id=\"droppable\"]"));
        Actions action = new Actions(driver);

        action.dragAndDrop(dragBox, dropBox).build().perform();
        driver.switchTo().defaultContent();
```





- - - -
## Drag and Drop
```java
driver.switchTo().frame(driver.findElement(By.xpath("//*[@id=\”content\”]/iframe")));

WebElement dragBox, dropBox;
dragBox = driver.findElement(By.xpath("//*[@id=\"draggable\”]"));
dropBox = driver.findElement(By.xpath(""//*[@id=\”droppable\”]"));
Actions action = new Actions(driver);

action.dragAndDrop(dragBox, dropBox).build().perform();


Thread.sleep(3000);
driver.close();

```

dragAndDrop()에서 drag할 element와 drop할 element를 지정해주면 됨.

- - - -
iframe에서의 확인이 끝났으면, 다시  switchTo().defaultContent()로 전환해주면됨.
page에서 iframe이있을때 다시 원래 페이지로 돌아가는 것.


