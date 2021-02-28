---
layout: post
title: Switching the Tests with Maven profiling
image:
categories: Maven
tags:
  - Maven
  - Java
---

어떠한 테스트케이스. 특정 케이스만 실행하고 싶을 때, 모든 케이스를 실행하고 싶을때, 이러한 핸들링을 어떻게 하면 적절하게 할 수 있을지?

- - - -



```xml
<profiles>
    <profile>
        <id>Regression</id>       
    </profile>
</profiles>
```

`pom.xml` 파일에 위와같은 태그를 생성하고. profile에 대한 아이디를 생성함.
그 다음, build태그를 내부에 nesting을 해줌.

```xml
<profile>
    <id>Smoke</id>
    <build>
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
    </build>
</profile>

```

profiles안에 여러개의 profile을 생성하고, id로 관리함.

`pom.xml` 에서 적절한 profiling 을 지정해주어서 관리할 수 있다.

`testng.xml` 파일은 이름이 굳이 `testng` 일 필요가 없으며 목적에 맞게 수정해줘도 무방할 것 같다.

바람직한 것은 하나의 suite레벨에, 하나의 xml파일이다.

https://www.qaautomation.co.in/2019/04/building-multiple-maven-profile-in-testng.xml.html

---



실행은 `mvn test -p {id]`
p는 profile.

인텔리제이에서는, 프로파일을 선택하고 실행하면됨.
![](/assets/posts/Maven/Switching%20the%20Tests%20with%20Maven%20profiling/578FD980-E623-4208-93B3-C852A9FF94C2.png)



