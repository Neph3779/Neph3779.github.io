---
layout: post
title: Reuse components
image:
categories: TestNG
tags:
  - TestNG
  - Java
---

Base가 될 class를 생성하고, 다른 테스트 클래스에서 해당 베이스 클래스를 extends하는 방식이 바람직함.

아니면 해당 메소드를 **static** 으로 선언을 해야한다.

어디선가 똑같이 기능하는 것들을 하나로 모아서 base로 만든 다음에 상속 / 구현하는 것이 좋다.

이를 위해서는 적절한 parameter와 return이 필요하다.

