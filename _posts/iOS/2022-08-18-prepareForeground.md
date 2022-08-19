---
layout: post
title: "[번역] Preparing Your UI to Run in the Foreground"
categories: [iOS]
tags: 
  - foreground
  - app life cycle
sitemap:
  changefreq: daily
  priority: 1.0
---

## 개요

앱의 UI가 화면에 띄워지는 것을 준비하기 위해 foreground 전환을 사용합니다. foreground로의 상태변환은 일반적으로 유저의 액션에 대한 응답입니다. 예를 들어 유저가 앱의 아이콘을 터치하면 시스템은 앱을 실행하고 foreground 상태로 만듭니다. 앱의 UI 업데이트, 리소스 요청, 유저의 요청을 핸들링하는 서비스의 시작을 위해 foreground로의 상태변환을 사용할 수 있습니다.

모든 상태 전환시에는 UIKit이 알맞은 delegate 객체에 notification을 보내게 됩니다. (appDelegate, sceneDelegate)

## Update Your App's Data Model when Entering the Foreground

앱을 실행할 때 시스템은 inactive 상태의 앱을 foreground 상태로 보냅니다. 앱의 launch-time 메서드를 활용해 해당 시점에 진행해야 할 작업을 할 수 있습니다. 앱이 background에 있었다면 scene을 지원하는 경우엔 `sceneWillEnterForeground` 메서드를 통해, 그 외의 경우에는 `applicationWillEnterForeground` 메서드를 통해 앱이 inactive 상태에 진입한 사실을 알립니다. background에서 foreground로의 전환시에 위의 메서드들을 사용해 리소스를 불러오거나 네트워크로부터 데이터를 가져올 수도 있습니다.



## Configure Your User Interface and Initial Tasks at Activation

시스템은 앱의 UI가 띄워지기 직전에 앱을 active 상태로 보냅니다. 이때는 앱의 UI를 구성하고 실행시간에 해야할 일들을 하기에 좋습니다. 다음과 같은 작업들이 그 예시입니다. 

- 앱의 window를 보여주기 (필요하다면)
- 보여지고 있는 viewController를 변경하기 (필요하다면)
- 데이터값이나 뷰, 컨트롤의 상태 변경하기
- dispatch queue를 시작하거나 재개하기

사용할 delegate에 맞게 `sceneDidBecomeActive`  또는 `applicationDidBecomeActive`을 사용하여 코드를 구성하면 됩니다.

앱 실행 시점은 유저에게 보여질 UI를 완성하는 시점입니다. 이때 activation 메서드를 block할만한 작업을 하면 안됩니다. 비동기적으로 데이터를 fetch하는 동안 보여줄 existing 데이터를 준비하는게 좋습니다.



##  Start UI-Specific Tasks when Your View Appears

activation 메서드가 끝나면 (return 되면) UIKit은 visible한 윈도우를 보여줍니다. 또한 보여줄 뷰와 관련된 이벤트를 notify합니다. `viewWillAppear` 메서드를 사용하여 최종적인 인터페이스 업데이트를 진행할 수 있습니다. 이 시점에 보여줄 ViewController를 변경한다든가 인터페이스에 중요한 변경을 진행해선 안됩니다. viewController가 onscreen 상태인 시점에는 인터페이스가 보여질 준비를 마친 상태여야 합니다.



## 마무리

크게 중요한 내용이나 얻어가는 내용은 없던 것 같습니다.

preparing Your UI to Run in the Background 문서는 먼저 정독한 뒤 필요하다면 번역글을 올리도록 하겠습니다.