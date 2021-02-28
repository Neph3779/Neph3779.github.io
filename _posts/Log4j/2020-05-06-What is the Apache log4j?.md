---
layout: post
title: What is the Apache log4j?
image:
categories: Log4j
tags:
  - Log4j
  - Java
---

Reliable, fast and flexible logging framework. Written in java.

특정 패키지 혹은 클래스에서의 정보를 확인할 수 있다.

mac의 경우, 홈페이지에서 tar.gz를 다운받을 수 있는데,  `api ` 와 `core`만 있어도 충분하다.

maven repo에서 다운받아주면 된다.

- - - -
일단 로그를 기록해줄 오브젝트를 선언한다.
선언할 때 파라미터로는 해당 클래스의 위치(정보)를 넘겨줘야한다.

```java
private static Logger log = LogManager.getLogger(Log4jDemo.class.getName()); //create Object

```

- - - -
```java
log.debug(“I am debugging”); //check information
log.info(“object is present”); //infomation
log.error(“object is not present”); //error message
log.fatal(“this is fatal”);

```

여기서  기본적으로는 error와 fatal밖에 표시안되는데, configuration 파일이 없기 때문. 설정이 필요함.

## when to use testing.
- Error() : to log when elements are not displayed in the page or if any validations fail
- Debug() : When each Selenium action is performed like click, SendKeys, getText()
- Info() : When operation is successfully completed ex: After loading page, or after any successful validations

