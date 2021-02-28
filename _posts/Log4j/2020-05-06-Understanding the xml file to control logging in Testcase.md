---
layout: post
title: Understanding the xml file to control logging in Testcase
image:
categories: Log4j
tags:
  - Log4j
  - Java
---



log4j는 모든 Java Framework에서 사용할 수 있음.


## Log4j Level
- Fatal : 심각한 에러가 발생.
- Error : 요청을 처리하는 중 문제가 발생한 상태.
- Warn : 처리 가능한 문제이지만 향후 시스템 에러의 원인이 될 수 있는 경고성 메세지
- Info : 로그인, 상태변경
- Debug : 개발 시 디버그 용도
- Trace : log4j의 상세내용.
Trace에 가까워질 수록 하위 레벨의 로그들이 전부 보여짐.

## Log4j Structure
- Logger (Category) : 로그레벨을 가지고 있음. 로그의 출력여부는 로그문의 레벨과 로거의 레벨로 결정.

`log4j2.xml`

```xml
<Loggers>
    <Root level=“error”>
        <AppenderRef ref=“Console”/>
    </Root>
</Loggers>

```
Root는 전체 레벨을 지정.


- Appender : 로그의 출력위치를 결정. (파일, 콘솔, DB등)
- PatternLayout : 출력되는 로그의 형식을 지정할 수 있음.
- `Loggers` 태그에 `name` attribute를 지정해줄 수 있는데, 특정 클래스에서의 로그레벨을 따로 지정해줄수도 있다.
- 
```xml
<Appenders>
    <Console name=“Console” target=“SYSTEM_OUT”>
        <PatternLayout pattern=“%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n”/>
    </Console>
</Appenders>
```

특정 package, class에서의 로그만 확인하고 싶을때는?
`Loggers`의 `Logger` 에서 좀 더 세분화시킬 수 있음.

- - - -
같은 로그가 2개씩 출력된다면, `additivity=“false”` 를 사용해볼 것.
Root Level도 해당 logger가 지정된 클래스를 다시 실행하기 때문에 중복실행을 방지하기 위한 태그.




[Log4j의 정의, 개념, 설정, 사용법 정리](https://cofs.tistory.com/354)
[Set log4j in Intellij IDEA 2019 and configuration of log4j.xml – Ahmed Waheed](https://mrcreamio.wordpress.com/2019/07/06/set-log4j-in-intellij-idea-2019-and-configuration-of-log4j-xml/)
