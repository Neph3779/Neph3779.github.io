---
layout: post
title: Prioritizing the Testcase
image:
categories: TestNG
tags:
  - TestNG
  - Java
---

테스트 케이스에 실행 우선순위를 적용시켜보자.

## 

어떠한 특정 테스트케이스만 실행하고 싶다면?

`testNG.xml` 파일에 추가로 작성해주기만 하면 됨.

```xml
<?xml version=“1.0” encoding=“UTF-8”?>
<!DOCTYPE suite SYSTEM “http://testng.org/testng-1.0.dtd”>
<suite name="All Test Suite">
    <test verbose="2" preserve-order="true"
          name="TestNG1">
        <classes> <!-- module -->
            <class name="section18.RunningTestcases"/>

        </classes>
    </test>
    <test name="TestNG2">
        <classes>
            <class name="section18.PrioritizingTestcase">
            </class>
        </classes>
    </test>
</suite>

```

위 코드에서는 `name= "TestNG2"` 라는 새로운 테스트케이스를 하나 추가해 주었고, 해당 테스트케이스에서 실행될 java파일도 class로 하나 선언해주었다.

test는 어떠한 실행목적, 실행기능, 실행메뉴 단위로 선언하여도 문제는 없다. 위와같이 한다면, `testNG.xml` 은 위에서 부터 아래로 실행되기 때문에, 저절로 나중에 추가한 테스트케이스가 나중에 실행될 것 이다.