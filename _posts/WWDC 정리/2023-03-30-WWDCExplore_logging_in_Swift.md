---
layout: post
title: "[WWDC] Explore logging in Swift 정리"
image:
categories: [WWDC 정리]
tags: 
  - WWDC
  - OS_LOG
  - 로그
sitemap:
  changefreq: daily
  priority: 1.0
---

**WWDC 영상 링크:** [Explore logging in Swift](https://developer.apple.com/videos/play/wwdc2020/10168/)

### Log의 장점

어떤 버그들은 정말 간헐적으로 발생하여 reproduce하기 어려운데

log가 남아있다면 해당 버그가 어떤 경로를 통해 발생했는지를 알기가 무척 쉬워집니다.



### Log의 특징

로그는 이벤트가 발생한 시점에 저장되며, 디바이스에 저장되어 나중에 검색하거나 다시 볼 수 있습니다.

또한 성능적으로 오버헤드가 매우 적어 부담없이 사용할 수 있습니다.



## Logging하는 방법

```swift
import os

let logger = Logger(subsystem: "com.neph.loggApp", category: "giftcards")

func beginTask(url: URL, handler: (Data) -> Void) {
  launcTask(with: url) {
    handler($0)
  }
  logger.log("Started a task")
}
```

os 모듈을 import한 뒤 logger instance를 생성합니다.

이때 subsystem과 category는 log들을 구분하기 위한 구별자로, 

일반적으로 subsystem에는 bundle identifier를 기입합니다.



### print와 다른점?

log 메시지는 print에서 출력되는 문장과 다르게 log 메시지는 string으로 전환되지 않습니다. (성능을 위해 fully convert 되지 않음)

log message를 직접 display할때만 string으로 변환되며, data 처리에 최적화 되어있습니다.



### Log 남길 수 있는 것들

CustomStringConvertible을 채택하는 모든것에 대해 로그를 남길 수 있습니다.

Int, Double같은 Numeric부터, 옵젝씨 객체 등 다양한 것들에 대해 로그를 남길 수 있으며

custom 객체도 위의 프로토콜을 채택하게 하여 로깅이 가능합니다.



### 주의사항

run time에 발생하는 data는 Numeric이 아니라면 log가 자동으로 삭제합니다.

이는 개인정보를 로깅하지 않도록 막기 위한 작업입니다.



### 변수를 logging

```swift
logger.log("Paid with back account \(accountNumber)")
```

위처럼 작성된 코드에서 accountNumber라는 변수는 private 처리되어 보이지 않습니다.

이때 privacy를 조절해서 보이게할수도 있으며 Log level 설명 파트에서 더 자세히 설명하겠습니다.



### 남겨진 로그를 보는 방법

터미널에서 log collect 커맨드를 실행해서 보거나 맥의 Console(콘솔) 앱을 실행시켜 GUI를 통해 살펴볼 수 있습니다.



### 구현 팁

버그는 어떠한 일련의 작업을 하다 발생하기 마련입니다.

로그를 filtering해서 살펴보기 위해 Task마다 id(UUID 등)를 달아주면

어떤 작업을 하다 발생한 에러인지 파악하기가 한결 수월해집니다.



### Log level

Log level은 다음과 같습니다.

- Debug
  - Non-Persistence하다는 특징을 가지며, 실행중에만 확인할 수 있는 log입니다.
- Info
  - log collect command가 실행되기 조금 전의 내용까지만 저장되는 log입니다. (부분적 Non-Persistence)
- Notice (디폴트)
  - logger.log를 사용했을때 기본적으로 실행되는 동작이며, Notice부터 그 아래의 level은 모두 Persistence하여 로그가 저장됩니다.
- Error
- Fault

로그의 레벨에 따라 콘솔앱에서 가시적으로 보이는 마크가 추가되기도 합니다.

로그는 디바이스의 용량에 따라 저장되는 양이 결정되며, 로그 저장소가 가득 차면, 오래된 로그 중 중요도가 낮은 로그부터 차례로 삭제됩니다. 따라서 일반적으로 로그는 며칠동안 저장됩니다.

여기서 Debug 레벨은 swift의 sophisticated optimization 덕에 무거운 함수를 사용하더라도 사용자에게는 성능적으로 악영향을 끼치지 않습니다. (사용자가 앱을 실행할때는 해당 동작이 코드로 작성되어 있더라도 아예 실행되지 않기 때문)



### Formatting

Xcode 콘솔에서 로그를 확인할때 formatting을 통해 데이터를 보기 좋게 가공해서 볼 수 있습니다.

align을 통해 가변적인 길이의 데이터를 줄맞춰서 볼 수도 있고

fixed(precision:)을 통해 소숫점 아래 자리를 제한해서 출력할수도 있습니다.



### Privacy

```swift
logger.log("Paid with bank account: \(accountNumber, privacy: .private(mask: .hash))")
```

위와같이 privacy를 설정하여 로그에 변수를 남길때 보여지게 할지 말지의 여부를 결정할 수 있습니다.

로그는 디바이스를 가진 누구나 확인할 수 있기 때문에 유저의 개인 정보를 public으로 두지 않는 것은 매우 중요합니다.

하지만 해당 정보를 통해 무언가를 식별할 일이 있다면 hash를 통해 값을 해싱하여 검색함으로써 해결할 수 있습니다.

