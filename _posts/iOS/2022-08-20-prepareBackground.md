---
layout: post
title: "[요약] Preparing Your UI to Run in the Background"
categories: [iOS]
tags: 
  - background
sitemap:
  changefreq: daily
  priority: 1.0
---

## 개요

- 앱은 여러 이유로 background 상태로 전환됨
- UIKit이 suspend하기 전에 앱은 background 상태로 가게 됨
- 시스템은 앱을 directly하게 background 상태로 실행할 수 있음 (suspended 상태의 앱을 background로 보내는 것도 가능함)
- background 상태의 앱은 최소한의 일을 해야 함
- foreground에서 background로 갈때 공유자원을 release하고 작업을 중단해야 함
- 만약 background로 보내져도 지속해야 할 작업이 있다면 최대한 빠르게 처리 해야 함



## Quiet Your App upon Deactivation

- 시스템은 여러 이유로 앱을 deactivate 시킴
- foreground에 있던 앱이 background로 바뀌는 경우에 background로 가기 직전에 deactivate 시킴
- 시스템 alert 띄우는 등의 상황에서 앱을 일시적으로 deactivate 시킬 수 있음
- system panel이 띄워져서 deactivate된 경우 유저가 panel을 dismiss시키면 앱을 자동으로 reactivate 시켜줌
- deactivate시에 UIKit은 scene 지원 여부에 따라 `sceneWillResignActive` 또는 `applicationWillResignActive`를 호출시켜줌



- deactivation을 통해 유저의 데이터를 보존하고 앱에서 진행되는 major한 작업을 모두 중단시켜야 함
- 아래는 중단해야할 작업들의 예시
  - 유저 데이터를 disk에 저장하고 open된 파일을 close시킴
  - dispatch와 operation queue를 suspend시킴
  - active 상태의 timer를 invalidate 시키기



## Release Resources upon Entering the Background

- 앱이 background로 갈때 메모리를 free 시켜주고 공유자원을 release 시켜줘야 함
- foreground에서 background로 갈때 메모리 free 작업은 매우 중요함
- foreground에서 실행중인 앱이 메모리 priority를 가지기 때문에 메모리가 부족할때 background에 있는 앱을 terminate 시켜서 메모리를 충당하는 경우가 있기 때문
- 물론 foreground에서 background로 간 경우가 아니더라도 최소한의 resource를 쓰도록 성능체크를 하는 것이 필요함
- foreground에서 background로 진입할 때 UIKit은 scene 지원 여부에 따라 `sceneDidEnterBackground` 또는 `applicationDidEnterBackground`를 호출시켜줌
- background 전환시에 아래의 작업을 최대한 진행해야 함
  - 파일로부터 직접 읽어들인 이미지나 미디어 파일의 discard
  - 디스크로부터 다시 읽어올 수 있는 대용량의 in-memory object의 discard
  - 카메라나 다른 하드웨어 자원으로의 접근을 release
  - 비밀번호와 같이 민감한 정보를 interface에서 숨기기
  - alert이나 임시 interface를 dismiss 시키기
  - 시스템 데이터베이스와의 연결 끊기
- 앱의 asset에서 name을 통해 불러와 사용하는 이미지는 discard하지 않아도 됨
- 이와 비슷하게 NSCache 객체를 관리하기 위해 사용하는 NSDiscardableContent 프로토콜을 채택한 객체는 release하지 않아도 됨 (시스템이 자동으로 cleanup 시켜주기 때문)
- background로 갈 때 앱이 시스템 공유자원을 가지고 있으면 안됨 (이런 경우 시스템이 앱을 terminate 시킴)
- 만약 리소스에 접근하기 위한 시스템 프레임워크를 사용중이라면 프레임워크의 문서나 가이드라인을 통해 처리방법을 숙지해야함



## Prepare Your UI for the App Snapshot

- 앱이 background에 진입하고 delegate 메서드가 return되면 현재 앱의 UI를 캡쳐함
- 해당 이미지를 app switcher에 띄워줌
- 또한 이 이미지를 foreground로 다시 진입할때 임시적으로 띄워줌
- 이 캡쳐된 UI에는 민감한 정보가 담겨서는 안됨
- background로 진입할 때 이 정보들을 view에서 삭제할 필요가 있음
- 또한 alert, 임시 interface, 시스템 view controller와 같이 앱의 화면을 잘 보이지 않게 하는 요소도 dismiss 시켜줘야 함
- 앱이 다시 foreground로 돌아왔을 때 데이터나 view를 다시 적절하게 복구시켜주면 됨



## Respond to Important Events in the Background

- 앱은 background로 진입할 때 일반적으로는 추가적인 실행 시간을 갖지 못함
- 하지만 time-sensitive한 일을 할 경우 UIKit은 추가적인 실행시간을 허용해주기도 함
- background 기능을 제공하는 앱의 경우 Xcode상에서 background mode capability를 활성화 시켜줘야 함
- 백그라운드 작업에 관한 것은 [Background Tasks](https://developer.apple.com/documentation/backgroundtasks) 문서를 참조