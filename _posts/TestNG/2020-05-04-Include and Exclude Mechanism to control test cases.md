---
layout: post
title: Include and Exclude Mechanism to control test cases
image:
categories: TestNG
tags:
  - TestNG
  - Java
---

테스트 케이스에서 특정 테스트 케이스 혹은 테스트 클래스에서 특정 테스트 메소드만 제외하거나, 추가하고 싶을 수도 있다.

`testNG.xml` 에서 comment처리 하는 것으로도 대응할 수 는 있으나 주석처리말고도 제대로 된 문법도 존재한다.



## Exclude
어떠한 특정 테스트케이스 (메소드) 만 제외하고 싶을 때.
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
                <methods>
                    <exclude name="mobileLogin"></exclude>
                </methods>
            </class>
        </classes>
    </test>
</suite>

```

`<methods>` 태그 내부에서 `<exclude>` 태그를 사용한다.
이때 메소드의 이름을 직접 적어줘야 한다.



## Include

반대로 `include` 태그는 해당 태그에 포함된 메소드만 실행하고 나머지 메소드는 실행하지 않는다.

사용방법은 `exclude`  태그와 동일하다.

- - - -


## Executing the Testcases at Package level with Regex

어떠한 40개의 모바일 테스트케이스와, 40개의 웹 테스트케이스가 있다고 가정 했을때, 웹만 실행한다고 할 때 나머지 모바일 40개를 전부 exclude 태그를 작성한다던지, include태그로 40개의 웹 테스트 케이스를 전부 작성하는 것은 좋지 않음.
만일 테스트케이스 네이밍이 어떠한 포맷을 따르고 있다면 정규표현식을 이용해 특정짓기가 가능해진다.

```xml
<include name="mobile.*"></include>
```

위와 같이 mobile 이름이 들어간 모든 메소드 들을 포함시킨다.

---



추가로, 패키지 단위로 실행하고 싶다거나 제외하고싶다면?

```xml
<test>
	<packages>
		<package name=“package”/>
	</packages>
</test>
```

패키지 태그를 생성하여 실행함. 패키지 내부의 클래스와 메소드들이 전부 실행된다.


