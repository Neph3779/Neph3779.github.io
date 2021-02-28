---
layout: post
title: Template in writing Testcase for Business Scenario
image:
categories: Cucumber
tags:
  - Cucumber
  - Java
---

### Business Requirement

비즈니스 요구사항은 아래와 같은 흐름으로 정의할 수 있어야한다.

---

##### In order to (Achieve something) 

Business outcome. Goal. Requirement. ~~를 하기 위해, ~~을 위해,

##### As a (Role)

~~로써, 어떠한 권한을 가진 상태에서~

###### I want to (Do this)

무엇인가를 하고 싶다~

---

모든 requirement는 **한 두 문장으로 정의할 수 있어야**한다.

_Example_ :
**In order to** Pay credit card payment
**As a** Netbanking sole owner who have credit section access
**I want to** Navigate to credit card section, Enter amount and process my payment.

---

위와 같은 어떠한 Business Requirement를 충족하는 Testcase를 작성해야할 필요가 있다.

아래는 Cucumber에서 채용하고 있는 _[Ghrekin](https://cucumber.io/docs/gherkin/)_ 이라는 문법이다. 이 문법에 따라 Business Requirement가 서술되고, 이 문서를 바탕으로 Testcase를 작성할 수 있게 된다.

---

##### Given 
What you need to have to perform action, -Prerequisites. 
사전조건. 주어진 환경.

##### When
Performs action, action, 어떠한 것을 한다면~

##### Then
The desired outcome for the user. 
그렇다면~ 예상 결과.

- - - -
_Example testcases_ 1
Given : An account with zero balance
When : I navigate to credit card payment section and click on submit giving amount
Then : It should throw error message-funds.

- - - -
_Example testcase_ 2
Given : An account with sufficient balance who does not have credit card
When : I navigate to credit card payment section 
Then : you don’t have to access as your are error message



각 조건에 맞는 테스트케이스를 작성할 수 있게 된다.