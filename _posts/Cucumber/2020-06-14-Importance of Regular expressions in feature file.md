---
layout: post
title: Importance of Regular expressions in feature file
image:
categories: Cucumber
tags:
  - Cucumber
  - Java
---

StepDefinition의 정의가, Feature의 양이 많아서 힘들다면, 그냥 정의하지 않고 실행하면 에러 로그로 snippet을 정의하라고 출력이 될 것임.

거기서 복사해서 소스코드에 붙여넣고 구현해도 됨.

이것은 framework가 알려주는 것.
![](/assets/images/posts/Cucumber/Importance%20of%20Regular%20expressions%20in%20feature%20file/6DE1663B-64CD-4526-AC90-CB7129704B97.png)

- - - -

특정 데이터를 feature파일에서 사용한다고 했을 때.
```gherkin
Scenario: Home page default login
  Given User is on NetBanking landing page
  When User login into application with “John” and “4321”
  Then Homepage is populated
  And Cards are not displayed

Scenario: Home page default login
  Given User is on NetBanking landing page
  When User login into application with "Jin" and "1234"
  Then Homepage is populated
  And Cards are displayed

```

비정상 케이스와 정상 케이스.

그러나 이렇게 직접 내용에 써주었을때, 테스트케이스가 많아지면 많아질 수록 관리하기가 힘들어진다.
특히 `when` 절 같은 경우에는,  사람 이름과 번호만 다를 뿐, 다른 구문은 같다.
즉 파라미터만 다르고 나머지는 같다는 것.

```java
@When(“User login into application with {string} and {string}”)
public void userLoginIntoApplicationWithAnd(String arg0, String arg1) {
}

```

intellij는 자동으로 변수로 만들어준다. intellij에 의존하지 않고도, regular expression을 사용해서 더욱 더 가능성을 확장시킬 수 있다.


이미 구현되어있는 시나리오는 중복해서 사용할수도 있다.
`given`, `then`과 같이.



