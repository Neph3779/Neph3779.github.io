---
layout: post
title: "[번역] Managing Your App's Life Cycle"
categories: [iOS]
tags: 
  - life cycle
  - foreground
  - background
  - inactive
  - unattached
sitemap:
  changefreq: daily
  priority: 1.0
---

이번 글에서는 [Managing Your App's Life Cycle | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle) 를 번역해보았습니다.

사실상 글의 정독이 목적이고 중요도가 떨어진다 생각되는 부분은 번역하지 않았으니 정확한 내용은 공식문서를 참조해주세요



앱이 foreground나 background에 있을때 이를 알리고 시스템 관련 이벤트들을 컨트롤 합니다.



## 개요

앱의 현재 상태는 현재 무엇을 할 수 있고 없는지를 결정하는 요소입니다. foreground 상태의 앱은 유저가 사용중인 앱이므로 시스템 리소스의 우선권을 갖습니다. 반대로 background에 있는 앱은 적은 일을 해야 하기에 리소스를 거의 할당받지 못합니다. 앱의 상태변화가 일어나면 그에 맞는 대응을 해줘야 합니다.

앱의 상태 변화가 일어나면 UIKit은 appDelegate이나 sceneDelegate의 메서드들을 호출함으로써 상태 변화 사실을 알려옵니다.

이때 iOS13 이전/이후로 UISceneDelegate이 호출될지 UIApplicationDelegate가 호출될지가 나뉩니다. (더 정확히는SceneDelegate 지원 여부에 따라 달라집니다.)



## Respond to Scene-Based Life-Cycle Events

앱이 scene을 지원한다면 UIKit은 각각의 scene마다 개별적으로 life cycle event를 전달합니다. 유저는 하나의 앱을 여러개 띄울 수 있으며 각각을 보이거나 숨길 수 있습니다. 각각의 scene이 독립적인 life cycle을 갖기 때문에 각각의 scene은 서로 다른 상태에 있을 수 있습니다. 어떤 scene은 foreground에 있는 동안 다른 scene들은 background나 suspend상태에 있는 것처럼 말이죠.

<br/> <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220817150220.png" alt="An illustration showing the state transitions for a scene-based app. Scenes start in the unattached state and move to the foreground-active or background state. The foreground-inactive state acts as a transition state." style="zoom:50%;" />



위의 사진은 scene의 상태변화도입니다. 유저나 시스템이 앱의 새로운 scene을 요청하면 UIKit은 unattached 상태의 scene을 만들어줍니다. 유저가 요청한 경우에는 빠르게 foreground 상태에 진입시켜 화면에 띄워줍니다. 시스템 요청의 경우에는 일반적으로 background 상태로 진입시켜 작업을 진행할 수 있도록 합니다. (location 이벤트 등으로 앱을 백그라운드에서 작동되도록 띄우는 경우 등이 예시) 유저가 앱을 dismiss하면 UIKit은 해당 scene을 background 상태로 변경하고 이는 곧 suspended 상태로 바뀌게 됩니다. UIKit은 background나 suspended 상태에 있는 scene을 언제든 disconnect 하여 unattached 상태로 가도록 할 수 있습니다. (리소스 회수의 목적) 

<br/> scene 변경은 다음과 같은 작업을 할 때 사용합니다.

- UIKit이 scene을 앱과 연결시킬때 개발자의 초기 UI를 구성하고 scene에 필요한 데이터를 로드할 때
- foreground-active 상태로 상태변화가 일어날때 UI를 구서하고 유저와의 상호작용을 준비할 때 
- background 상태로 갈 때 중요한 작업을 끝마치고 확보할 수 있는 메모리를 최대한 확보하고 app의 snapshot에 대비할 때
- scene disconnection 시에 shared resoure를 정리할 때



## Respond to App-Based Life-Cycle Events

iOS 12 이전 혹은 scene을 지원하지 않는 경우에는 UIKit이 모든 life-cycle 이벤트를 UIApplicationDelegate을 통해 전달했습니다. app delegate은 앱의 모든 window를  관리합니다. 결과적으로 앱의 상태 변화는 UI에 영향을 끼칩니다.

<br/> <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220817222803.png" alt="An illustration showing the state transitions for an app without scenes. The app launches into the active or background state. An app transitions through the inactive state. " style="zoom:50%;" />

위의 그림은 app delegate 객체를 포함한 상태 변화도를 나타낸 것입니다. 앱 실행 이후에 시스템은 UI를 화면에 띄울지의 여부를 바탕으로 앱을 inactive나 background 상태로 보냅니다. foreground 상태에서 실행중인 경우에는 자동으로 앱을 active 상태로 보냅니다. 이후 앱이 terminate 될 때까지 상태변화를 반복합니다.

<br/> 앱의 상태변화는 다음과 같은 경우에 사용합니다.

- 앱의 실행시에 데이터 구조와 UI를 초기화할 때
- 실행중에 UI 구성을 마무리하고 유저와의 상호작용을 준비할 때
- 종료시에 데이터를 저장하고 앱의 behavior를 quiet 시킬때 (앱 마무리 한다는 뜻 같습니다.)
- background 상태로 진입할때 중요한 작업을 끝마치고 메모리를 최대한 확보하고 앱의 snapshot을 준비합니다.
- 앱의 종료시에 모든 작업을 중단하고 공유 리소스를 release합니다.



## Respond to Other Significant Events

life-cycle 이벤트를 handling하는 것에 추가로 앱은 아래의 목록에 대해서도 준비해야 합니다. 대부분 UIApplicationDelegate 객체를 사용해 관리하며 몇몇 경우에는 notification을 통해 관리해야 할수도 있습니다. 

- Memory warnings
  - 앱이 메모리 사용이 너무 과다할때 받는 이벤트(경고)입니다. 
- Protected data becomes available/unavailable
  - 유저가 디바이스를 lock, unlock할때 발생하는 이벤트입니다.
- Handoff tasks
  - NSUserActivity 객체가 생성되어야 할 때 받는 이벤트입니다.
- Time changes
  - 시간 변경등의 이유로 발생하는 이벤트입니다.
- Open URLs
  - 앱이 resource를 오픈할때 발생하는 이벤트입니다.
