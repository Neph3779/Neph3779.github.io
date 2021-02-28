---
layout: post
title: Importance of surefirePlugin in executing tests
image:
categories: Maven
tags:
  - Maven
  - Java
---

`surefire` 를 사용하게 되면 TestNG의 기능을 maven과 연계하여 사용할 수 있게 되는데, 그중의 몇 가지 기능은 아래와 같다.

3 main keyword.

_IntelliJ는 view > Tool window -> Maven project_ 를 열면 확인할 수 있음.

---



### mvn clean
class 파일을 포함하여, 각종 임시파일 들을 삭제함. 이전 실행하고 남은 파일들을 삭제함.
프로젝트 레벨에서 실행할 수 있음. `pom.xml` 이 위치하고 있는 프로젝트레벨에서만 실행할 수 있음.

### mvn complies
어떠한 코드에 문제가 없는지. 컴파일을 할 수 있는지 확인하는 것. Syntax error를 확인할 수 있음.

이후 문제가 없다면 compile을 진행하며, 여기서 class파일을 생성하게 된다.

### mvn test
compile를 한 후 실행. 이 명령어가 테스트를 실행하는 트리거가 된다.
test하기전에 complie을 하지 않으면, 자동으로 컴파일해주긴 함.
이 것을 시작하면 일단 pom.xml을 읽어들여 dependencies를 체크한다. 로컬의 .m2 레포지토리에 설치되어있는지 확인을 하고, 없으면 mvn repo에 접속하여 다운로드 받음.

- - - -
Mvn test는 java파일의 이름에 test가 들어간 것을 찾아내어 실행하기 때문에, test가 포함되어있지 않으면 maven이 찾아낼 수 없음.

➡ 즉, 파일이름에 `Test` 가 들어가게끔 작성을 해주어야한다.