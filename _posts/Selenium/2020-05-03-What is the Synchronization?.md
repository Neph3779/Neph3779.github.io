---
layout: post
title: 11. Synchronization - Wait
image:
categories: Selenium
tags:
  - Selenium
  - Java
---



## What is the Synchronization?

시스템의 Thread들이 수행되는 시점을 조절하는 것을, Synchronization 이라고 함.
동기화.



---

## Explicit wait

##### 명시적 대기. 

어떠한 결과를 위해서 명시적으로 코드로 선언해주는 것.
암시적인 대기를 해도 결과가 로드되지 않을 때에는, 셀레니움이 실행되면서 요소를 찾을 수가 없어 에러를 일으킬 수 있음.

이때에는 충분히 웹페이지의 요소가 로드된 후 실행될 수 있도록, 시간을 부여하여 명시적으로 대기할 수 있도록 해야함.

- - - -

`WebDriverWait wait = new WebDriverWait(driver, 5); `

_WebDrriverWait_ 라는 클래스를 호출하여 선언할 수 있는데, 초기값으로 driver의 값과 몇초를 슬립할 것인지에 대한 값을 넣어줌.

`wait.until(ExpectedConditions.visibilityOfElementLocated(By.cssSelector(“span.promoInfo”)));`

`until()` 에 `ExpectedConditions` 를 사용해줌. 여러 조합이 있는데, 대표적으로 `visibilityOfElementLocated()` 가 있음.
즉, 어떠한 요소가 위치해있는 곳을 읽거나 확인할 수 있는 상황이 될 때까지 기다린다는 것.

Implicit wait와의 차이점은, 어떠한 특정 요소에 대해 대기할 수 있다는 것.

퍼포먼스 이슈는 없음. 그러나 코딩량이 많아지고 선언하기가 좀 더 까다로워짐.



---



## Implicit Wait

##### 암시적/묵시적 대기

어떠한 검색 버튼을 눌렀을 때, 잠시 후에 검색결과가 나오는 그러한 것. 네트워크 상황이나 기타 이유로 인하여 로딩에 시간이 걸릴 때.
보통 get()을 사용하면 최대 3초까지 자동으로 대기함.

- - - -

어떠한 메소드를 실행하면, 실행결과가 전부 로딩될 때 까지 기다리는 것.


Wait time globally.

- - - -

driver에 implicity설정을 해줄 수 있는데, 초기 driver 설정 시에 이것을 선언(설정) 해줄 수 있음.
`driver.manage().timeouts().implicitlyWait(4, TimeUnit.SECONDS);`

`timeout()` 에서 `implicitlyWait()`  라는 메소드를 호출하여 원하는 숫자와 단위를 입력해줄 수 있다.

위 시간은 항상 위 시간대로 대기하는 것이 아니라, **최대 대기시간**이라는 개념.
바로 로딩되면 바로 다음단계로 넘어감.

**Readable code.**
퍼포먼스가 이슈가 있다고 하나 위 코드를 한 번만 설정해놓으면 매 줄마다 선언해줄 필요가 없음.



---



## Fluent Wait?

Explicit wait의 한 종류. (명시적 대기)

Explicit wait는 두 가지 종류가 있음.

1. WebDriverWait
2. FluentWait

Fluent Wait finds the web element repeatedly at *regular intervals* of time until the timeout or till the object gets found.
Unlike WebDriverWait, need to build Customized wait methods based on condition.
Both WebDriverWait and FluentWait classes implement Wait interface

- - - -

**Wait** some second and **polling** n sec.
Continually monitoring.

- - - -

```java
Wait<WebDriver> wait = new FluentWait<WebDriver>(driver)
                                .withTimeout(Duration.ofSeconds(30))
                                .pollingEvery(Duration.ofSeconds(3))
                                .ignoring(NoSuchElementException.class);

```

30초간 기다리며, 3초 후 다시 30초를 반복. 3초동안 요소가없는 예외가 발생하여도 무시함.

FluentWait는 아쉽게도, 어떠한 요소를 기다린후 캐치하도록 하는 지시메소드가 없음. 그래서 직접 구현해야함.

```java
WebElement text = wait.until(new Function<WebDriver, WebElement>() {
    @Override
    public WebElement apply(WebDriver driver) {
        return driver.findElement(By.id(“finish”));
    }
});

```

- - - -

실제 작업시에는 그렇게 유명하지도, 많이 쓰이지도 않지만 각종 매체 등에서 많이 소개된 내용임.





