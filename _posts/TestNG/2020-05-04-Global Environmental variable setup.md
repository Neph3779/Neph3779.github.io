---
layout: post
title: Global Environmental variable setup
image:
categories: TestNG
tags:
  - TestNG
  - Java
---



@DataProvider로 데이터를 불러오는 것은 좋으나, 해당 테스트케이스만이 아닌, 모든 테스트케이스에 적용하는 방법은…?

Java의 **Property** 클래스를 이용해볼 것.
```java
Properties prop = new Properties();
FileInputStream fis = new FileInputStream("/Users/yoodahun/Documents/Github/Java/Selenium WebDriver with Java/Framework_in_AutomationTest/TestNG/src/section18/dataDriven.properties");
prop.load(fis);

```

FileInputStream을 이용해서 properties 파일을 읽어들이고, Properties object의 `load()` 에 parameter로 넘겨줄 것.

그 다음, property파일을 생성해주고 그 내부에 키와 밸류형식의 데이터들을 선언해줄 것.

```properties
username=selenium
password=testpassword
url=https://www.google.com
browser=chrome
```

```java
Properties prop = new Properties();
FileInputStream fis = new FileInputStream(“/Users/~~dataDriven.properties”);
prop.load(fis);

WebDriver driver;

System.out.println(prop.getProperty("username"));
if(prop.getProperty("browser").contains("chrome")) {
    driver = new ChromeDriver();
} else {
	driver = new FirefoxDriver();
}   

```

