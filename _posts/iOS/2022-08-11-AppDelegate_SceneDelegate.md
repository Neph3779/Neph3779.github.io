---
layout: post
title: "AppDelegate란?"
image:
categories: iOS
tags: 
  - AppDelegate
  - SceneDelegate
sitemap:
  changefreq: daily
  priority: 1.0
---

## UIApplicationDelegate

appDelegate는 앱을 구동할때 공유되는 행위를 관리하는 객체입니다.

appDelegate가 하는 일은 다음과 같습니다.

- 앱의 자료구조의 초기화
- 앱의 Scene 구성
- 앱 외부로부터 오는 notification (배터리 부족, 다운로드 완료 등) 들에 대한 responding
- push notification을 통한 접속시 알맞은 서비스로 이동할 수 있도록 해줌



SceneDelegate의 경우 iOS13 이후 한 앱을 화면을 나누어 띄우는게 가능해지면서 Scene이란 개념이 추가되었고 이에 따라 기존에 UIAppDelegate에서 관리하던 몇몇 기능이 SceneDelegate으로 넘어간 것이므로 이번 글에서는 패스하도록 하겠습니다.



## Initializing the App

```swift
func application(UIApplication, willFinishLaunchingWithOptions: [UIApplication.LaunchOptionsKey : Any]?) -> Bool

func application(UIApplication, didFinishLaunchingWithOptions: [UIApplication.LaunchOptionsKey : Any]?) -> Bool

struct UIApplication.LaunchOptionsKey

class let didFinishLaunchingNotification: NSNotification.Name
```

willFinishLaunch 와 didFinishLaunch는 앱이 구동하기 직전, 직후에 호출되는 메서드이다.

이때 어떤 방법으로 앱이 켜졌는지를 LaunchOptionsKey 인자로 받아오므로 확인이 가능하다.

또한 notification으로도 앱이 켜졌음을 확인받을 수 있다.



## Configuring and Discarding Scenes

```swift
func application(UIApplication, configurationForConnecting: UISceneSession, options: UIScene.ConnectionOptions) -> UISceneConfiguration

func application(UIApplication, didDiscardSceneSessions: Set<UISceneSession>)
```

iOS 13 이후 Scene이 추가되면서 Scene의 생성, 삭제와 관련한 일을 appDelegate에서 맡게 되었다.



## Responding to App Life-Cycle Events

```swift
func applicationDidBecomeActive(UIApplication)

func applicationWillResignActive(UIApplication)

func applicationDidEnterBackground(UIApplication)

func applicationWillEnterForeground(UIApplication)

func applicationWillTerminate(UIApplication)

class let didBecomeActiveNotification: NSNotification.Name

class let didEnterBackgroundNotification: NSNotification.Name

class let willEnterForegroundNotification: NSNotification.Name

class let willResignActiveNotification: NSNotification.Name

class let willTerminateNotification: NSNotification.Name
```



앱의 state가 변환될때 호출되는 메서드, notification들이다. 상당히 직관적인 이름들이라 특별한 설명은 필요하지 않을 것 같다. 

SceneDelegate으로 기능이 넘어간 메서드로는

- willEnteForeground
- didEnterBackGround
- willResignActive
- didBecomeActive

가 있다.

