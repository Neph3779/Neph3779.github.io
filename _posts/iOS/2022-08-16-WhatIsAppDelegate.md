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

이번 글에서는 appDelegate에 어떤 메서드들이 존재하고 어떤 것들을 담당하는지에 대해 다뤄보겠습니다.

다음 글에서 appDelegate과 sceneDelegate이 분리된 이유에 대해 다뤄보도록 하겠습니다.

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

willFinishLaunch 와 didFinishLaunch는 앱이 구동하기 직전, 직후에 호출되는 메서드입니다.

이때 어떤 방법으로 앱이 켜졌는지를 LaunchOptionsKey 인자로 받아오므로 확인이 가능합니다.

LaunchOptionsKey에는 블루투스를 통한 이벤트, url을 통한 접속 등 다양한 값들이 존재합니다.

또한 notification으로도 앱이 켜졌음을 확인받을 수 있습니다.



## Configuring and Discarding Scenes

```swift
func application(UIApplication, configurationForConnecting: UISceneSession, options: UIScene.ConnectionOptions) -> UISceneConfiguration

func application(UIApplication, didDiscardSceneSessions: Set<UISceneSession>)
```

iOS 13 이후 Scene이 추가되면서 Scene의 생성, 삭제와 관련한 일을 appDelegate에서 맡게 되었습니다.

자세한 내용은 SceneDelegate을 다룰 때 추가하도록 하겠습니다.



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



앱의 state가 변환될때 호출되는 메서드, notification들이다. 상당히 직관적인 이름들이라 특별한 설명은 필요하지 않을 것 같습니다.

SceneDelegate으로 기능이 넘어간 메서드로는

- willEnterForeground
- didEnterBackGround
- willResignActive
- didBecomeActive

가 있습니다.



## Responding to Environment Changes

```swift
func applicationProtectedDataDidBecomeAvailable(UIApplication)

func applicationProtectedDataWillBecomeUnavailable(UIApplication)

func applicationDidReceiveMemoryWarning(UIApplication)

func applicationSignificantTimeChange(UIApplication)

class let protectedDataDidBecomeAvailableNotification: NSNotification.Name

class let protectedDataWillBecomeUnavailableNotification: NSNotification.Name

class let didReceiveMemoryWarningNotification: NSNotification.Name

class let significantTimeChangeNotification: NSNotification.Name

```

이 파트에서는 앱의 여러 환경 변화에 따라 호출되는 메서드들이 존재합니다.

protected data (아마 앱 내의 데이터베이스 등이 아닌가 싶습니다.)에 접근할 수 있게 되었을때,

앱 메모리 관련 warning이 발생했을 때에 notification이나 메서드를 통해 조작해줄 수 있습니다.



## Downloading Data in Background

```swift
func application(UIApplication, handleEventsForBackgroundURLSession: String, completionHandler: () -> Void)

enum UIBackgroundFetchResult
```

백그라운드를 통해 데이터를 받아오는 작업도 appDelegate에서 진행합니다.

이때 handleEvent... 파라미터는 identifier로 사용되면 URLSessionConfiguration의 init시에 사용할 수 있습니다.

백그라운드 작업과 관련된 내용은 [위 메서드의 공식문서](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622941-application)에 상세히 설명되어 있습니다.



## Handling Remote Notification Registration

```swift
func application(UIApplication, didRegisterForRemoteNotificationsWithDeviceToken: Data)

func application(UIApplication, didFailToRegisterForRemoteNotificationsWithError: Error)

func application(UIApplication, didReceiveRemoteNotification: [AnyHashable : Any], fetchCompletionHandler: (UIBackgroundFetchResult) -> Void)
```

서버에서 보내는 푸시(remote notification)에 대한 응답도 appDelegate에서 처리합니다.



## Providing a Window for Storyboarding

```swift
var window: UIWindow?
```

sceneDelegate을 사용하지 않는다면 앱의 윈도우 인스턴스도 appDelegate에서 만들어주어야 합니다.

일반적으로는 didFinishwithLaunchOption 메서드에서 window의 초기화, window.rootViewController의 지정을 해줍니다.

## Providing the Main Entry Point

```swift
static func main()
```

절대 건드릴 일은 없겠지만 main 메서드도 appDelegate에 존재합니다.

정말 앱이 시작되면 가장 먼저 호출되는 함수입니다.



## 마무리

appDelegate는 글에 정리한 것 이외에도 헬스킷, user activity 등 여러 다양한 기능을 담당하고 있었습니다.

이번 공부로 app life cycle의 변화에 따른 대처, 백그라운드 작업, push notification, remote notification 등 여러 처리를 appDelegate에서 한다는 것을 알 수 있었습니다.