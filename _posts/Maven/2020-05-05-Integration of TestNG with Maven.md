---
layout: post
title: Integration of TestNG with Maven
image:
categories: Maven
tags:
  - Maven
  - Java
---

`testNG.xml` 을 maven에서 인식하게 하여 maven에서 test를 실행할 수 있게 해야함.
Mvn test으로 테스트폴더의 테스트들을 실행시킬 수 있음.

- - - -
자동생성된 xml 파일은, 정렬되어있지 않을 수도있는데
Cmd + alt + L을 누르면 자동정렬

- - - -
testNG.xml을 maven으로 실행시키고자할때.

```xml
<pluginManagement>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>3.0.0-M4</version>
            <configuration>
                <suiteXmlFiles>
                    <suiteXmlFile>testng.xml</suiteXmlFile>
                </suiteXmlFiles>
            </configuration>
        </plugin>
    </plugins>
</pluginManagement>

```
Surefire 태그 안에 `configuration`태그를 사용하여 넣을 것. **원하는 xml파일을 실행시킬 수 있음**.

즉, 이 것으로 원하는 단위 Suite 레벨의 지정이 가능하다.특정 지정이 없으면 모든 테스트케이스를 실행하게됨.

- - - -
특정 자바파일만 실행시키고 싶을 땐?
`mvn -Dtest={java filename} test`

intelliJ에서 실행시키고자 할 때는, maven window에서 test를 오른쪽 클릭하고
Create ~~를 클릭한다음,  command line필드에 `-Dtest={java filename} test` 넣어주면 된다. 

