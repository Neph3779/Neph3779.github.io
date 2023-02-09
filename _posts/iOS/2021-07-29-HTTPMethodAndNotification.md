---
layout: post
title: "[Core] HTTPMethod.swift, Notification.swift 분석"
image:
categories: [iOS]
tags: 
  - Alamofire
  - HTTPMethod
  - Notification
sitemap:
  changefreq: daily
  priority: 1.0
---

## HTTPMethod

HTTPMethod는 구조체로 이루어져있다. 

RawRepresentable, Equatable, Hashable을 채택하고 있으며 rawValue가 String 타입이기 때문에 HTTPMethod를 String 요구자리에 가져다 써도 문제가 없다.

Hashable의 경우 Set\<HTTPMethod\>를 사용하는 부분이 있으므로 채택해주어야 하며, Equatable은 사용자 편의를 위해 채택한 것으로 보인다.



### enum이 아닌 struct인 이유

RawRepresentable 프로토콜의 요구사항인 rawValue를 처리하기엔 enum에 비해 struct가 용이하기에 struct를 택한 것 같다.



### 지원 Methods

Alamofire에서 지원하는 HTTPMethod는 아래와 같다.

- CONNECT
- DELETE
- GET
- HEAD
- OPTIONS
- PATCH
- POST
- PUT
- TRACE



## Notifications

Alamofire는 다양한 notification을 EventMonitor 객체를 통해 지원한다.



### 지원 Notifications

Almofire에서 지원하는 Notification은 아래와 같다.

#### Request를 담고 있는 Notification들

- didResumeNotification
- didSuspendNotification
- didCancelNotification
- didFinishNotification

#### URLSessionTask를 담고 있는 Notification들

- didResumeTaskNotification
- didSuspendTaskNotification
- didCancelTaskNotification
- didCompleteTaskNotification



Notification, EventMonitor 파트는 자세한 구현 내용을 알기보다는 어떤 상황에 어떤 Notification이 오는지만 알고 활용해도 충분할 것 같다.



[https://github.com/Alamofire/Alamofire/blob/master/Source/HTTPMethod.swift](https://github.com/Alamofire/Alamofire/blob/master/Source/HTTPMethod.swift)

[https://github.com/Alamofire/Alamofire/blob/master/Source/Notifications.swift](https://github.com/Alamofire/Alamofire/blob/master/Source/Notifications.swift)

