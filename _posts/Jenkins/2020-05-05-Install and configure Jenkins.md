---
layout: post
title: Install and configure Jenkins
image:
categories: Jenkins
tags:
  - Jenkins
  - Java
---

어떠한 특정한 서버에서 Jenkins를 실행시킨다.

특히, 회사에서 jenkins를 이용하고자한다면, 개인의 컴퓨터가 아닌 사측 infra용 서버나, 배포용 서버에 이미 구축이 되어있을 것이다.

개발팀이나 인프라팀에게 문의해보자.

로컬서버에서 실행하는 경우는 아래와 같다.

https://www.jenkins.io/download/lts/macos/

mac의 경우는 `homebrew`  을 이용하여 설치가능하다. window 의 경우도 다운로드 가능.

Java -jar Jenkins.war ? 로 실행.

- - - -
일단 IDE에서 maven project생성을 해주더라도 jenkins에 처음 등록하고자 할때는 내 컴퓨터에도 메이븐이 설치 되어야함.

메이븐 역시 로컬에 설치를 해주면된다.

---

접속할때에는 `localhost:8080` 으로 접속하면 될 것 같다.

Jenkins 를 설치하고 처음 접속할때는, 초기 비밀번호가 필요하다.

`Users/사용자/.jenkins/secrets/initialAdminPassword`

위와 같은 경로로 접속하여 초기 비밀번호를 확인해준다.

이후 로그인 한다음 첫 계정을 설정해주고 사용해야한다.

---



## Configuring Jenkins setting and workspace

### Add new job

New item에서 freestyle project를 추가한다.
로컬에서 작업하므로 로컬 패스를 지정해주나, git이 있다면 git주소를 소스코드 관리영역에서 추가해주면 된다.

### Build peridically

입력해주면 주기적으로 build를 진행. 없으면 전혀 실행하지 않음.

---



## TestNG Reporting plugin into Jenkins jobs

TestNG의 Surefire는 test Result를 xml파일로도 생성하는데
testResult.xml의 내용을 Jenkins에다가 연결할수도 있다.

### Plug-in setup

우선 Jenkins 에서 플러그인을 설치해주어야 한다.

Jenkins home ➡ System Configuration ➡ Management Plugin ➡ TestNg Results plugin download

- - - -

## Plug-in setting

설치한 후에는 앞서 생성한 job의 설정에서 post-build actions에 접속하여 testNG reports를 선택하여준다.
기본옵션으로 설정하고 실행하면 결과가 나옴



#### Reference

https://nesoy.github.io/articles/2017-02/Jenkins