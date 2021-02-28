---
layout: post
title: Testcase logging with log4j example
image:
categories: Log4j
tags:
  - Log4j
  - Java
---



## Export to file
`AppenderRef` 태그의 ref값을 원하는 속성의 name으로 변경시켜주면 출력가능함.

```xml
<RollingFile name="File" fileName="${basePath}/" filePattern="${basePath}/prints-%d{yyyy-MM-dd}.log">
    <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
    <SizeBasedTriggeringPolicy size="500" />
</RollingFile>

```

- fileName : log가 저장될 파일이름. 만약 파일이 존재하지 않는다면 생성함.
- filePattern : 용량을 초과해서 새로운 파일이 만들어지면, 기존 파일의 네이밍을 지정해줌.
- SizeBasedTriggeringPolicy : 해당 용량만큼 log파일이 만들어지면, 새롭게 로그파일을 만드는 것을 지정.

```xml
<Properties>
	<Property name=“basePath”>./src/logs</Property>
</Properties>
```
위와 같이 어떠한 경로를 Property 태그를 이용하여 지정해줄 수도 있음.

- - - -

완성판
```xml
<?xml version=“1.0” encoding=“UTF-8”?>
<Configuration status=“WARN”>
    <Properties>
        <Property name="basePath">./logs</Property>
    </Properties>
    <Appenders>
        <RollingFile name="File" fileName="${basePath}/" filePattern="${basePath}/prints-%d{yyyy-MM-dd}.log">
            <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
            <SizeBasedTriggeringPolicy size="500" />
        </RollingFile>

        <Console name="Console" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
        </Console>
    </Appenders>
    <Loggers>
        <Root level="trace">
            <AppenderRef ref="File"/>
        </Root>
    </Loggers>
</Configuration>

```

