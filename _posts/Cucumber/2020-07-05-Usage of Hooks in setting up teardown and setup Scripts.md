---
layout: post
title: Usage of Hooks in setting up teardown and setup Scripts
image:
categories: Cucumber
tags:
  - Cucumber
  - Java
---



Hook 은 StepDefinition이 포함되어있는 package와 같은 level에서 작성되어야한다.

- - - -
## @Before
모든 시나리오가 실행되기 전에 실행된다.
그렇다면 `background` 키워드와 다른 점은 무엇인가?
-> 모든 시나리오가 실행되기 전에 실행되는데, 어떠한 태그를 골라서 실행시킬 수 있음.

```java
@Before(“@MobileTest”)
public void beforeValidation(){
    
}
```
이렇게 하면, `@MobileTest` 라는 태그를 가진 테스트케이스가 실행되기 전에만 실행됨.
`@Before` 태그의 파라미터로는, 태그명을 지정해주면 되는데, 더블쿼테이션으로 감싸주면 된다.

- - - -
## Hook과 Background는 같이 쓰일 수가 없다.

-> Hook은 어떠한 조건에 해당되는 테스트케이스만 따로 핸들링하기 위해서 쓰이는 것이지만, background는 해당 feature file에 존재하는 모든 시나리오에 실행되기 때문.

![](/assets/images/posts/Cucumber/Usage%20of%20Hooks%20in%20setting%20up%20teardown%20and%20setup%20Scripts/B879E517-DC7B-4BBB-A57A-5E29D5C3A9EA.png)
같이 쓰인 것을 알 수 있는데, 이 순서가 뒤섞일 수도 있다고함.

- - - -
## @After
Before와는 반대로, 어떠한 시나리오가 끝난 후에 실행됨.


