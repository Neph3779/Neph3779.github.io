---
layout: post
title: Include and Exclude Mechanism to control test cases
image:
categories: TestNG
tags:
  - TestNG
  - Java
---

TestNG는 Annotation 을 이용하여 여러 기능을 사용할 수 있다.



### @BeforeTest
어떠한 테스트를 진행할때, 데이터가 필요할 것인데 어떤 테스트를 반복할 때 해당 테스트를 지우고 다시 input하고 이것을 반복하기에는 번거로움.

TestNG에 어떠한 테스트를 실행하기전에 데이터를 정리하는 것을 지정해줄 수 있음.

뿐만아니라, 이 annotation을 사용하면 특정 테스트 혹은 클래스를 실행하기 전에 실행될 메소드를 지정해줄 수 있음.

```xml
<test verbose=“2” preserve-order=“true”
      name=“TestNG1”>
    <classes> <!— module —>
        <class name=“section18.RunningTestcases”/>
        <class name=“section18.AnnotationTest”/>
    </classes>
</test>

```
`AnnotationTest` 에 `@BeforeTest` 가 선언되어있다고 가정해보자.

위 상황의 경우, `AnnotationTest`가 나중에 실행되지만, @BeforeTest로 인하여 `AnnotationTest`  파일에서 에서 `@BeforeTest` 가 붙은 메소드가 가장 제일 실행된다.

---

### @AfterTest

반대로 테스트 단위에서 제일 마지막에 실행하고자 하는 메소드에는 `@AfterTest` 를 선언한다.



- - - -
### @BeforeSuit, @AfterSuit
TestSuit의 전체를 커버함.
어떠한 환경변수를 설정하고 해제할때 사용하면 좋음.



- - - -
### @BeforeMethod @AfterMethod
모든 메소드가 실행되기 전후로 실행된다.

추가적으로, TestMethod는 같은 클래스 내부의 메소드의 경우, **알파벳 순서** 로 실행됨. 



- - - -
### @BeforeClass @afterClass

모든 클래스(java file)이 실행되기 전후에 실행된다.



- - - -
## Annotation helper attribute
### @Test(dependsOnMethod = {methodName} )

어떠한 메소드에 의존적으로, 어떤 메소드가 실행된 후 실행 되도록 함.
위 annotation에 선언한 메소드가 실행되지 않으면, 실행되지 않음.
이것을 사용하면 method이름의 알파벳순서대로 실행되는 테스트들의 순서를 바꿀 수가 있음.
컴마로 구분하여 여러개의 메소드를 추가로 선언할 수도 있음.



### @Test(Enabled=false)
어떠한 메소드를 무시하는 것. 테스트 대상의 어떠한 문제때문에 계속 false가 일어나고, 레포트에 빨간색 불이들어온다면, 이 annotation을 이용하여 무시해줄 수도 있음.
이미 report한 문제라면, 해당문제가 해결될때까지 skip하여도 될 것임.
만약 interview에서 어떻게 테스트 케이스를 무시하겠냐고하면? Annotation helper attribute의 `enabled` attribute를 사용하여 스킵하겠다고해야 올바른 대답이라고 할 수 있다.



### @Test(Timeout = x)
어떠한 테스트를 지정한 x시간동안 충분히 실행될 수 있도록 기다리는 것.









