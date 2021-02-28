---
layout: post
title: Listeners interface in TestNG framework.
image:
categories: TestNG
tags:
  - TestNG
  - Java
---



## What is TestNG Listeners?
어떠한 테스트 전후를 기점으로 활성화되는 것.
예를들어, 테스트가 실패한다면 스크린샷을 찍는다고 하자면, 테스트가 실패하는 경우를 캐치할 수 있어야한다. Listener는 이러한 것들을 자동으로 처리해준다.



## ITestListeners
TestNG에는 `ITestListeners` 라는 interface가 있어서 이것을 구현하면 된다.

이후, xml파일에서 Listeners의 위치를 등록해야한다. 그래야만 TestNG에서 Listener의 존재를 파악하게된다.

```xml
<listeners>
    <listener class-name=“section18.Listeners”/>
</listeners>

```



