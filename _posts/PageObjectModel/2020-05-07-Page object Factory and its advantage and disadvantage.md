---
layout: post
title: Page object Factory and its advantage and disadvantage
image:
categories: Page-Object-Pattern
tags:
  - Page Object Pattern
  - Java
---

Page Object Factory란, Selenium에서 지원하는 Page Object Model을 좀 더 구현해주기 쉽게하는 일종의 기능.

## Advantage
- Easy to Maintain
- Easy Readability of scripts
- Reduce or Eliminate duplicity
- Re-usability of code
- Reliability

## Disadvantage
- **High Setup Time & Effort**: Initial effort investment in development of Automation Framework is high. This is the biggest weight of POM in case of web applications with hundreds/thousands of pages. It is highly suggested that if this model is decided to be implemented, then it should be done parallel to development of the application. Refer  [V-Model](http://www.tutorialspoint.com/sdlc/sdlc_v_model.htm)  for Software Development Life Cycle.
  - 페이지가 많은 웹 어플리케이션을 구현하고자하면 시간이 많이 소요됨. 초기 단계에서 개발과 동시에 진행되지 않으면 힘듦.
- **Skilled labor**: Testers not technically sound or aware of programming best practices are a nightmare in this case. Perhaps this is the biggest mistake to make, employing unskilled labor in hopes of training them during implementation. Unskilled testers need to undergo a Training Boot Camp to be ready for such an undertaking. Also the Architecture of the framework should be defined clearly and completely before development in order to avoid any loopholes in later stages. Every application is different and it may require the automation framework to be significantly tailored towards it.
  - 프로그래밍에 어느정도 숙련도가 있어야한다. POM이란, 설계에 필요한 디자인 패턴이므로, 설계에도 어느정도 지식이 있어야하며 상황별로 유연하게 대처할 수 있는 능력이 필요하다.
  - 만약 프로그래밍 경험이 없는 사람이 테스트 자동화를 비롯하여 POM을 구현하려고 한다면 시간이 많이 소요될 것이다.
- **Specific**: Not a generic model. Automation Framework developed using POM approach is specific to the application. Unlike keyword-driven/data-driven frameworks, it is not a generic framework.
  - POM은 일반적으로 널리 알려진 개발방식은 아니다.

