---
layout: post
title: 13. Handling Multiple Windows
image:
categories: Selenium
tags:
  - Selenium
  - Java
---

Handling Multiple Windows



인터넷창에서 새로운 창이 열렸을때는?

```java
//Return handling id
    Set<String> ids = driver.getWindowHandles();
```

`getWindowHandles()` 는 Set을 return하게 되는데, 윈도우 객체들의 id가 들어있다.

첫번째는 부모 윈도우.
두번째는 자식 윈도우
… 이런식으로 증가하게 됨.

```JAVA
//Return handling id
    Set<String> ids = driver.getWindowHandles();
    Iterator<String> id = ids.iterator();

    String parentId = id.next();
    String child = id.next();

    driver.switchTo().window(child);


```
윈도우의 정보를 가져오면, Set의 String으로 가져오게되는데, 이것을  iterator로 바꾸어 원하는 값을 찾을때까지 next()를 해주면 됨.
이후, switchTo().window()를 해주면 바라보는 창이 달라짐. 이것을 하지않으면, 보이는 창은 가장 마지막에 열린 창이 될 수 있으나, 동작은 이전 윈도우에서 계속해서 될 가능성이 있다.

Child window에서 parent로 바꾸기 위해서는 다시 `switchTo()` 를 해주면됨.

- - - -
close()는 윈도우를 닫아주고,
quit()는 프로세스를 종료해줌. 프로그램 종료.

- - - -



## Assignment

```java
WebElement clickHere, text;
String parent, child;

clickHere = driver.findElement(By.xpath("//*[@id=\"content\"]/div/a"));
clickHere.click();

Iterator<String> windowsIterator = driver.getWindowHandles().iterator();
parent = windowsIterator.next();
child = windowsIterator.next();

driver.switchTo().window(child);
text = driver.findElement(By.xpath("/html/body/div/h3"));
System.out.println(text.getText());

driver.switchTo().window(parent);
text = driver.findElement(By.xpath("//*[@id=\”content\”]/div/h3"));
System.out.println(text.getText());

```

위와 같은 방식으로 윈도우를 변경할 수 있다.



---

## Get the Title each every tabs.

일단 특정 윈도우나 탭으로 driver를 switch한 다음, getTitle()을 하면 됨.

```java
//get the title every child tabs.
String clickOnLinks = Keys.chord(Keys.COMMAND, Keys.ENTER);
for(int I = 1; I < firstColumnLinks.size(); I++) {
    firstColumnLinks.get(i).sendKeys(clickOnLinks);
    //get the title every child tabs.

    Thread.sleep(5000);
}
Set<String> windows = driver.getWindowHandles();
Iterator<String> it = windows.iterator();

while(it.hasNext()) {
    driver.switchTo().window(it.next());
    System.out.println(driver.getTitle());
}


```

`while()` 에서 Iterator의 다음 요소가 존재한다면, 계속해서 창을 바꾸어주면서 참조할 수 있다.

Iterator를 사용. 어떠한 집합은 순서가 없지만, iterator를 사용하면 순차적으로 돌릴 수 있게됨.

