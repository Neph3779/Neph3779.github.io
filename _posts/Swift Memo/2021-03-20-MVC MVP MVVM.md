---
layout: post
title: "MVC, MVP, MVVM 차이"
image:
categories: Swift Memo
tags: 
  - 디자인패턴
  - MVC
  - MVP
  - MVVM
sitemap:
  changefreq: daily
  priority: 1.0
---

### MVC MVP MVVM 차이 제대로 알아보기

- #### MVC

  Model이 하는 일

  1. data를 가지고 있음

  View가 하는 일 (UIView)

  1. Controller에게 명령받아 View를 띄워줌

  Controller가 하는 일

  1. Model의 data 꺼내와서 로직 처리
  2. 사용자 Input 받아오기
  3. View에게 View를 띄우도록 명령 (UIViewController, IBAction)

  <br/>

   **MVC의 단점 : Controller가 너무 많은 것을 담당함.**

  <br/>

- #### MVP

  Model이 하는 일

  1. data를 가지고 있음

  View가 하는 일 (UIViewController, IBAction)

  1. Input, Ouput 처리
  2. 들어온 Input 무조건 Presenter에게 전달
  3. Presenter가 지시한 Output 보여줌

  Presenter가 하는 일

  1. View가 보내준 Input을 통해 로직 처리
  2. View에게 Output 지시

  <br/>

  Presenter(MVP)와 Controller(MVC)의 차이점: 

  ​	Presenter는 화면에 대한 정보를 가지고 있지 않음, View와 Presenter는 1대1의 관계

  <br/>

  **MVP의 단점: View마다 Presenter가 붙어야 하기 때문에  코드가 길어짐 (비슷한 로직은 공통된 함수에서 처리한다고 쳐도 View에서 들어온 Input을 받아서 로직을 처리해서 View에게 쏴주는 작업을 할 Presenter가 View마다 필요)**

  <br/>

- #### MVVM

  View가 하는 일

  1. ViewModel을 Observe
  2. ViewModel의 내용이 변경되면 이를 자체적으로 반영

  ViewModel이 하는 일

  1. Model의 Data 가공, 처리 및 변경

  Model

  1. Data를 관리

[위 내용 작성을 위해 참고한 링크](https://www.youtube.com/watch?v=bjVAVm3t5cQ&ab_channel=%EA%B3%B0%ED%8A%80%EA%B9%80)

<br/>