---
layout: post
title: "06. Handling Dropdown"
image:
categories: Selenium
tags:
  - Selenium
  - Java
---

Handling Dropdown element.

### Dropdown Element가 Static인지 Dynamic인지 확인하는 방법은...?

개발자도구를 열어서 확인함. 

➡ select 태그이면 static일 확률이 높음…???

---

## Handling Static dropdown

```java
Select s = new Select(Element);

slectByValue(“”);
selectByIndex();
selectByVisibleText(“”);

```

위 Select기능은 select tag에만 사용할 수 있음.

---

## Handling Dynamic Dropdown

Dynamic dropdown은 어떠한 값을 선택한 다음에 표시됨. 

➡ 즉, 먼저 선택한 값에 따라 표시되는 값이 달라짐.

경우에 따라 똑같은 값이 한 페이지안에 여러개 표시될 수도 있음.
이럴 때는 어떻게해야하는지?? 
➡ 명시적으로 원하는 순서의 요소를 선택하도록 입력을 해주어야함.

즉, 어떠한 트리거가 되는 값을 선택한 후, dropdown의 값을 확인하는, 하나하나 확인하는 형태로 되어야함.

---

Dynamic dropdown에서 값을 확인할 때는,

Xpath 전체를 ()로 감싸고, 뒤에 배열처럼 인덱스를 지정해주어야함.
첫번째값은 1로 시작.
(//a[@value=\”MAA\”])[2]

---

Dynamic dropdown은 서버로부터 데이터값을 읽어들이기 때문에
충분한 시간을 부여하여 데이터 로드를 기다리고 해야함.
*Implicity Wating*

---

## Handling Auto Suggestive Dropdown

어떠한 값을 입력하면, 제일 확률이 높은 것을 제시해주는 기능이기 때문에 테스트하고자하는 값을 우선 입력하고 나오는 값을 키보드 입력등을 이용해서 확인하는 것이 좋음.
중간중간 sleep을 넣어주어야 좋음.

WebElement.sendKey()에
Key값을 넣어서 사용할 수 있음 _downArrows_

---

### Handling Auto suggestive dropdown option.

Option은 개발자도구로 소스를 보려고해도 볼 수가 없음. 자동적으로 사라짐.
키보드 이벤트를 이용하여 오브젝트를 이동해가며 텍스트를 하나하나 확인하는 것이어야함.

**Selenium은 Hidden text를 감지할 수가 없음**

---

### Importance of Javascript Executor

Selenium 에서 hidden element를 불러내기 위해서 JavascriptExecutor라는 것을 사용할 수 있음.

```java
String target = "BENGALURU INTERNATION AIRPORT";
JavascriptExecutor javascriptExecutor = (JavascriptExecutor)driver;
String value = “return document.getElementById(\”fromPlaceName\”).value;”;

String text = (String)javascriptExecutor.executeScript(value);

while (!text.equals(target)) {
    System.out.println(text);
    inputBox.sendKeys(Keys.ARROW_DOWN);
    text = (String)javascriptExecutor.executeScript(value);
    Thread.sleep(2000);
}
inputBox.sendKeys(Keys.ENTER);
```

Script를 문자열형식으로 지정해줄 필요가 있음.
이때 javascript DOM을 이용하여 auto suggestive를 하는 input의 값을 얻어올 수 있는데,  return까지 지정해주어야함.

---

findElement()의 getAttribute(“value”);를 해도 할 수 있음.

```java
WebElement inputbox = driver.findElement(By.xpath("//*[@id=\"autocomplete\"]"));
while(!inputbox.getAttribute(“value”).equals(country)) {
    System.out.println(inputbox.getAttribute(“value”));
    inputbox.sendKeys(Keys.ARROW_DOWN);
    Thread.sleep(2000);
}
inputbox.sendKeys(Keys.ENTER);
System.out.println(inputbox.getText());
```
