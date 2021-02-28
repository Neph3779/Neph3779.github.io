---
layout: post
title: Why TestNG and Its Advantage
image:
categories: TestNG
tags:
  - TestNG
  - Java
---

## Why TestNG and Its Advantage

Control Testcases.

테스트 케이스들을 관리하기 좋으며 각종 수치 계측 및 테스트 결과가 올바른지, 문제가 있는지 판단하기 쉽기 때문. 

테스트 케이스들은 TestSuite로 관리할 수 있음.



---

## Running testcases in TestNG without java compiler

TestNG는 Java기반으로 돌아가긴 하나, 소스코드는 무조건 main 클래스가 존재해야할 필요는 없음.

그러나 그냥 실행하려고 하면 main클래스가 없다고 뜨기 때문에, 메소드 앞에 `@test`를 붙여서 선언해야함.

```java
import org.testng.annotations.Test;

//Section18-135 Running testcaes in TestNG
public class RunningTestcases {
    @Test
    public void Demo() {
        System.out.println("hello");
    }
}

```

testNG.xml을 이용해 run application을 하지 않아도 실행 대상 Suite, TestClass, TestMethod 들을 관리할 수 있음.



---

## Importance of xml file in testNG configuration

하나의 클래스에 여러개의 testcase(Method)를 정의할 수 있음.
어떻게해야 여러 개의 테스트케이스를 정의하고 다르게 구현할 수 있을까?

```java
@Test
public void Demo() {
    System.out.println(“hello”);
}

@Test
public void Demo2nd(){
    System.out.println(“world !”);
}


```

단순하게 테스트 어노테이션을 두개 선언해주면 된다.

- - - -

XML file이면 특정 케이스들을 한꺼번에 실행시켜줄 수 있다.
Xml 파일 내부에서 필요한 것들을 선언.

##### TestNG.xml 

```xml
<?xml version=“1.0” encoding=“UTF-8”?>
<!DOCTYPE suite SYSTEM “http://testng.org/testng-1.0.dtd”>
<suite name=“All Test Suite">
    <test verbose="2" preserve-order="true"
          name="/Users/yoodahun/Documents/Github/Java/Selenium WebDriver with Java/Framework_in_AutomationTest/TestNG”>
        <classes> <!— module —>
            <class name=“section18.RunningTestcases”>
            </class> <!-- module -->
        </classes>
    </test>
</suite>

```

위와 같이 정의한다면, `classes` 내부의 `class` 는, 다시 class 내부에 있는 모든 method 들이 실행대상이 된다는 것이다.

`TestNG.xml` 파일은 프로젝트의 루트계층에서 접근할 수 있다.(프로젝트 최상단에 위치해야만 함.)

