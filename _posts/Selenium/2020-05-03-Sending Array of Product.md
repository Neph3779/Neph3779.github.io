---
layout: post 
title: 10. Sending Array of Product
image:
categories: Selenium
tags:
  - Selenium
  - Java
---

Sending Array of Product

아무런 스타일이 붙어있지 않는 요소를 눌러야할때는 어떻게해야할지…??

예를 들어 쇼핑몰 홈페이지에서 스타일이 붙지않아 특정지을수 없는 버튼이 여러개 있을 경우에는?
[GreenKart - veg and fruits kart](https://rahulshettyacademy.com/seleniumPractise/#/)

- - - -
for문을 돌려가며
어떠한 상품을 `contains()` 로 문자열을 입력하여 찾아낸 다음,
해당 인덱스의 버튼을 클릭하는 방법이 있을 것 같다.

```java
for (int i = 0; i < elements.size(); I++) {
    String name = elements.get(i).getText();

    if (name.contains(“Cucumber”)) {
        driver.findElements(By.xpath("//button[text()=‘ADD TO CART’]")).get(i).click();
    } else {
        continue;
    }
}
```

위와 같은 방법을 이용하여 특정지을 수 없는 element를 찾아내어 어떠한 행위를 할 수 있다.

- - - -


예상되는 어떠한 문자열을 미리 문자의 배열로 지정하여  for문을 사용하여 매치하는지 안하는지를 확인할 수도 있다.

문자열을 문자열 리스트로 변환하면, contains를 사용할 수 있음.
```java
List<WebElement> elements = driver.findElements(By.cssSelector(“h4.product-name”));
String[] itemsNeeded = {“Cucumber”, “Brocolli”, “Beetroot”};
List itemsNeededList = Arrays.asList(itemsNeeded); // convert String to List
Thread.sleep(3000);
for (int i = 0; i < elements.size(); i++) {
    String name = elements.get(i).getText().split("-")[0].strip(); //하이픈 제거

    if (itemsNeededList.contains(name)) {
        System.out.println(name);
        driver.findElements(By.xpath("//button[text()=‘ADD TO CART’]")).get(i).click();
        Thread.sleep(1000);
    }
}


```

예상되는 어떠한 값들을 List로 저장해놓고 값을 찾아낸다면 동작하게 하는 방식이다.



단 문제가 있는데, 비교하는 문자열에, 비교하고자하는 단어가 다른 단어와 함께 포함되어있을 경우에는 제대로 비교가 안될 수 있다. 완전 매치가 아니기 때문이다. 이럴때는 `split()`  과 `strip()`을 사용하여 단어를 깨끗하고 완전한 매치를 할 수 있는 상태를 만들어주는 것이 좋다.



- - - -

Debugging에서는 제대로 되던것이 일반 실행때는 제대로 실행이 안될때가 있음.

테스트 웹 페이지에서는, 버튼을 클릭하면 버튼의 텍스트가 바뀌는데, for문 안에 `findElements()` 를 실행하고 있으므로, for문이 실행될 때마다 요소의 갯수가 변하는 것. for문에 밖에서 확인해주어야함.

```java
List<WebElement> elements, buttons;
elements = driver.findElements(By.cssSelector(“h4.product-name”));
buttons = driver.findElements(By.xpath(“//button[text()=‘ADD TO CART’]”));

String[] itemsNeeded = {"Cucumber", "Brocolli", "Beetroot"};
List itemsNeededList = Arrays.asList(itemsNeeded); // convert String to List
Thread.sleep(3000);
for (int i = 0; i < elements.size(); i++) {
    String name = elements.get(i).getText().split("-")[0].strip();

    if (itemsNeededList.contains(name)) {
        System.out.println(name);
        buttons.get(i).click();
        Thread.sleep(1000);
    }
}


```

**왠만하면 text로 확인하는 것은 좋지 않다...** text는 동적으로 변하는 값들이 많기 때문!!


