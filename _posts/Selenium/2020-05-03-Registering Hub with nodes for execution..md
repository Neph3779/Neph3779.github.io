---
layout: post
title: 23. Registering Hub with nodes for execution.
image:
categories: Selenium
tags:
  - Selenium
  - Java
---

이전 [포스트]({% post_url 2020-05-03-Introduction to Grid %})에서 계속.



## Registering Hub

https://www.selenium.dev 에서 Grid용의 jar파일을 다운받음 
그 다음 터미널을 실행시킨 후, jar파일의 위치로 이동하여 아래 커맨드를 실행함.
`java -jar selenium-server-standalone-3.141.59.jar -role hub`

(_다운 받은 jar파일의 이름에 따라 커맨드는 적절하게 수정해주어야한다._)

이렇게하면 위 커맨드를 실행한 컴퓨터가 **Hub**역할을 하게됨.



---

## Registering Node

다른 컴퓨터에서 마찬가지로 selenium-server-standalone을 다운받아 준 후, 위와 같은 커맨드를 한다. 다만 아래 커맨드로 수정해야한다.
`-role WebDriver -hub {hub ip} {-port}(port 5566?)` 
노드로 등록하는 것.

hub의 ip를 입력하고 포트는 임의의 포트를 지정해준다. 기본 5566이면 될지도?

- - - -
`http://192.168.3.2:4444/grid/console` 에서 노드의 상태를 확인할 수 있음.

- console
- register : 등록용

- - - -
Hub에 Node를 추가했다면 준비가 된 것 이다.
그 다음부터는 node의 chromedriver를 인식시켜야함.

1. Node에 driver를 설치해야함.
2. `java -Dwebdriver.chrome.driver=“C:/chromedriver.exe” -jar ~~~`
로 실행해줌. 이때 jar파일과 driver는 같은 폴더에 있는 것이 좋은 것 같다.…?
위 커맨드를 노드실행 및 등록할때 같이 해줌. 즉, `-jar` 뒤에 node등록 커맨드를 같이해주면 될 듯.



- - - -
## DesiredCapabilities Class
Grid를 이용하여 테스트를 할 때는 위 클래스를 이용하여 설정해줄 수 있음.
```java
    //DesiredCapabilities.

    DesiredCapabilities dc = new DesiredCapabilities();
    dc.setBrowserName(“Chrome”);
    dc.setPlatform(Platform.WIN10);
    
//Testing remotely.
    WebDriver driver = new RemoteWebDriver(new URL("http://localhost:4444/wd/hub"), dc);

```

마지막의 url은 hub의 url.



