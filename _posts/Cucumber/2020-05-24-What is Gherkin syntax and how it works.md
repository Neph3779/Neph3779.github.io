---
layout: post
title: What is Gherkin syntax and how it works
image:
categories: Cucumber
tags:
  - Cucumber
  - Java
---

저번 [포스트]({% post_url 2020-05-23-Template in writing testcases for Business Scenario %}) 에서 계속.



## Gherkin

It is a Business *Readable*, *Domain specific language* that lets you describe **software’s behavior**
보통의 business requirement는 Dev와 QA에서 각각 이해하기에 다르게 이해할 수도 있음.
그렇기 때문에, Business Analyst가 공통된 **format**의 문서를 명확하게 작성하여 Dev와 QA의 이해를 맞춰야할 필요가 있게됨.

Gherkin은 그러한 공통된 format을 지원하는 Cucumber의 툴? 기능? 이라고 생각하면 쉽다.

---

## Keywords used in Cucumber 

Gherkin에는 다음과 같은 문법이 있다.

Scenario, Feature, Feature file, Scenario outline, Step definition

### Scenario
In Cucumber testcases are represented as Scenario.
Scenario contains Steps which are equivalent to test steps and use the following keywords (Gherkin syntax) to denote them. Give, When, But, Then
	- Given : Preconditions are mentioned in the Given keyword
	- When : The purpose of the when Steps is to describe the user action
	- Then : The purpose of Then Steps is to observe the expected output. The observations should be related to the business value / benefit of your Feature description


어떠한 시나리오에 근거하여 테스트를 작성할 수 있어야한다.
만일, BA가 위와 같은 형식으로 요구사항을 제공해주지 않는다면, cucumber를 이용할 수 없게되는데, 이 경우에라도 Cucumber를 이용하고 싶다하면 직접 Scenario keyword format에 맞추어 변형을 해야한다.

When we specify a business requirement , sometimes there are multiple pre-condition, user actions, and expected outcome.

We are going to add one more Scenario and will use the And and But keywords.
	- And : This is used for statements that are an addition to the previous Steps and represent positive statement.
	- But : This is used for statements that are an addition to previous Steps and represent negative statement.

여러개의 조건들이 온다면, `and` 로 묶을 수 있고, 부정적인 테스트케이스가 온다면 `but` 으로 표현할 수 있다.

### Feature
Feature represents  Business requirement.
Feature file acts as a Test Suite which consist of all Scenarios.

In Cucumber, Feature files contain Scenarios. We can simply create feature file with `Feature` extension. Scenarios belonging to specific area of Application will be grouped into one feature file.

The text that immediately follows the Feature keyword, and is in the same line. Is the Title of the Feature file.

Feature file should contain either Scenario or Scenario outline. The naming conventions for feature files should be lowercase with `feature` extension.

`feature`  파일에서의 각각의 문장은, 하나의 테스트케이스 (테스트 메소드)로 표현을 해야한다.








