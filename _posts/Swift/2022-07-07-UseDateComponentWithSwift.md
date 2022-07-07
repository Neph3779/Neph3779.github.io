---
layout: post
title: "Swift로 날짜 다루기 [DateComponent 편]"
image:
categories: Swift
tags: 
  - 날짜 다루기
  - Date
  - DateComponent
sitemap:
  changefreq: daily
  priority: 1.0
---

Swift로 날짜 다루기의 마지막편인 DateComponent편입니다.

DateComponent는 date의 요소들을 확장할 수 있도록 캡슐화 된 객체입니다.

특정 시간, 어떤 시간 크기 (ex. 5시간 16분) 등을 나타낼 수 있습니다.

DateComponent는 DateComponent가 가지고 있는 모든 field를 채우기를 요구하지는 않습니다.



## Init

```swift
init(calendar: Calendar?, timeZone: TimeZone?, era: Int?,
     year: Int?, month: Int?, day: Int?, hour: Int?,
     minute: Int?, second: Int?, nanosecond: Int?, weekday: Int?,
     weekdayOrdinal: Int?, quarter: Int?, weekOfMonth: Int?,
     weekOfYear: Int?, yearForWeekOfYear: Int?)
```

Date Component의 Init은 여러가지 요소들을 optional type으로 받습니다.

인자로 전달하지 않은 값은 nil로 세팅됩니다.

위의 인자로 전달하는 프로퍼티 값 중 `calendar`와 `timeZone`값은 직접 접근할수도 있습니다.



## Validate

DateComponent를 통해 날짜가 유효한지(이게 날짜가 맞는지) 등도 확인할 수 있습니다.

`isValidDate`라는 연산 프로퍼티를 통해서 Boolean 값을 받을수도 있고

`isValidDate(in: Calendar)`라는 메서드를 통해 전달받을수도 있습니다

`date`라는 `Date?` type의 프로퍼티도 존재하여 optional값을 unwrap하여 유효성을 확인할수도 있습니다.



참고로 Calendar 객체에서도 `date(from: DateComponents) -> Date?` 라는 메서드를 지원합니다.



## 날짜 세부요소

DateComponent는 그 이름에 걸맞게 여러가지 날짜 세부요소를 제공합니다

제공하는 목록은 다음과 같습니다



### Months and Years

- era
- year
- yearForWeekOfYear
- quarter
- month
- isLeapMonth



### Accessing Weeks and Days

- weekOfMonth
- weekOfYear
- weekday
- weekdayOrdinal
- day



### Accessing Hours and Seconds

- hour
- minute
- second
- nanosecond



위의 모든 값들은 `Int?` type입니다.



## Calendar Component

DateComponent 내의 값은 `Calendar.Component` 의 값과 같은 정보를 담은 값이 많습니다.

enum type인 `Calendar.Component`를 인자로 받아 DateComponent 내의 알맞은 값을 세팅하는 메서드도 지원합니다.

```swift
func value(for: Calendar.Component) -> Int?
func setValue(Int?, for: Calendar.Component)
```



## DateComponent 값의 비교

Calendar와 마찬가지로 `==` 연산과 `!=` 연산을 지원합니다.





## 최종 정리

이번 공부로 Swift로 날짜를 다루는 여러가지 방법이 존재함을 알게되었습니다.

DateComponent는 말 그대로 Date와 관련된 여러 값들을 가지고 있는 객체,

Date는 `TimeInterval` 이란 단위로 수치화된 요소들을 담고 있으며, 

formatting시에 사용하기 좋은 여러 메서드, enum을 가지고 있는 객체,

마지막으로 Calendar는 날짜의 비교 및 여러 Symbols들을 적절히 활용하기에 좋은 객체라는 사실까지 알게되었습니다.

