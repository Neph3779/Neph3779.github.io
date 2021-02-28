---
layout: post
title: Parameterizing from TestNG
image:
categories: TestNG
tags:
  - TestNG
  - Java
---

`TestNG.xml` 파일에서 파라미터를 핸들링 할 수 있다.

Api key, login정보, 어떠한 특정 URL등은 모든 테스트케이스에서 쓸 수 있기에 parameterizing을 하는 것이 좋다.
- - - -
```xml
<parameter name="URL" value="https://www.naver.com"></parameter>
```
이러한 name과 value가 있는 형식으로 `parameter` 태그를 지정해줘서 사용할 수 있다.
선언하는 위치에따라 grobal, local 변수로도 사용할 수 있다.

```xml
<suite name=“All Test Suite”>
    <parameter name="URL" value="https://www.naver.com"></parameter>
    <test verbose="2" preserve-order="true"
          name="TestNG1">
<!--        <groups>—>
<!—            <run>—>
<!—                <include name=“Smoke”></include>—>
<!--            </run>-->
<!--        </groups>-->
        <classes> <!-- module -->
            <class name="section18.RunningTestcases"/>
            <class name="section18.AnnotationTest"/>
        </classes>
    </test>


```

지금은 suite level 에서 선언하였으므로 어떠한 테스트케이스에서도 사용할 수 있다.
test단계나 class에서 선언하면 locally 하게 사용할 수 있다.
locally하게 사용하면 name은 똑같이 사용하면서 value는 다른값을 사용할수도 있을 것이다.

- - - -

위와 같이 선언한 parameter를 사용하기 위해서는
```java
@Parameters({"URL"})
@Test(timeOut = 4000)
public void ascending(String url) {
    System.out.println(“asceding”);
    System.out.println(url);

}

```
특정한 메소드의 annotation으로 `@Parameters` 를 선언하고, ({})에서 문자열로 name을 입력해준다.
그다음 메소드의 parameter명을 선언해주고 사용하면 된다. 메소드의 parameter값으로 xml에서 선언한 parameter값이 들어간다고 이해하면된다.



- - - -

여러가지 파라미터가 필요할 경우에는, xml파일에 선언해주면 되고
Method에 컴마로 구분하여 key(name)값을 기재해준다.

```java
@Parameters({“URL”, “APIKey/Username”})
@Test
public void ascending(String url, String username) {
    System.out.println("asceding");
    System.out.println(username);
    System.out.println(url);
}
```

xml파일에 선언한 파라미터는, xml파일에 의존적이게되므로 반드시 xml파일 레벨에서 테스트케이스를 실행해야한다. (테스트 클래스, 메소드만으로 단독으로 실행할 경우, 제대로 실행되지 않음.)



- - - -
그러나 대량의 데이터가 필요할 때는 어떻게 할 것인지?
대량의 데이터가 필요한 테스트케이스는…?
Parameterizing with multiple data sets by running tests with multiple combination??


### @DataProvider
```java
@DataProvider
public Object[][] getData() {
    // 1. username , password
    // 2. another username, password
    // 3.
    Object[][] data = new Object[3][2];
    data[0][0] = "first username”;
    data[0][1] = "first password";
    data[1][0] = "Second username";
    data[1][1] = "Second password";
    data[2][0] = "Third username";
    data[2][1] = "Third password"

    return data;
}

```
데이터를 제공하는 annotation.

```java
@Test(dataProvider = “getData”)
public void mobileLogin(String username, String password){
    System.out.println("mobileLogin");
    System.out.println(username);
    System.out.println(password);
}

```
제공되는 데이터를 사용하는 테스트메소드.
이렇게 하면 위 메소드로 입력되는 데이터는 3 row를 가지고있으므로 3번 실행됨.



