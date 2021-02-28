---
layout: post
title: Running Tests in parallel and Generating reports
image:
categories: TestNG
tags:
  - TestNG
  - Java
---



테스트 케이스가 만약 실패한다면, 어느 구간에서 어떠한 메소드에서 실패하였는지,
 Instance(Object) 의 어떠한 값이 문제가 되었는지를 알 수 있어야한다. 그래야 Debug를 진행하고 테스트케이스를 수정한다거나, 테스트 대상의 코드를 수정할 수 있게 된다.

[Listeners interface in TestNG framework]({% post_url 2020-05-04-Listeners interface in TestNG framework %})

##### Listener

```java

@Override
public void onTestFailure(ITestResult iTestResult) {
    System.out.println(“Test Failed!!”);
    System.out.println(iTestResult.getName());
    System.out.println("----------------------------");
}

```
`getName()` 을 하게된다면 실패한 메소드의 이름을 확인할 수 있다.



- - - -
## Testing parallel.
병렬적으로, 동시다발적으로 테스트를 실행할 수도 있으나, 실제로는 추천하지 않음. 퍼포먼스 이슈가 발생할 수 있다. (ex. 테스팅하는 프로그램의 서버능력, 테스팅하는 컴퓨터의 메모리 사용량, 네트워크 문제 등등..)

Suite level에서 `parallel` 속성 추가 후 `Thread-count` 속성을 추가한다. 몇 개의 테스트 케이스를 동시에 실행하느냐에 따라 thread의 수를 다르게 기재해주어야 한다.

`<suite name=“All Test Suite” parallel=“tests” thread-count=“2”>`

Parallel에 test가 아닌 class를 지정해줄 수도 있음. 위 코드라면 2개의 test 단위를 동시에 진행한다는 것이다.

- - - -
### Reports
테스트 결과후 프로젝트의 루트 계층에서 `test-output` 폴더를 보면 간단한 report를 확인할 수 있다.

만약, IntelliJ 기준으로, 생성되지 않았다면, Edit configurations > Listeners tab을 찾아 Use default listeners 를 체크하도록 하자.

참고 사이트 : https://stackoverflow.com/questions/15457499/intellij-with-testng-report-generation