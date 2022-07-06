---
layout: post
title: "Swift로 날짜 다루기 [Calendar 편]"
image:
categories: Swift
tags: 
  - 날짜 다루기
  - Date
  - Calendar
sitemap:
  changefreq: daily
  priority: 1.0
---



이번에는 Date와 밀접한 Calendar 객체에 대해 살펴보겠습니다.

# Calendar

Calendar는 calendar 유닛과 특정 시점간의 관계를 정의합니다.

date의 계산과 비교를 위한 여러 기능을 제공합니다. 

## Init

```swift
let calendar = Calendar(identifier: .japanese)
```

Calendar는 identifier를 통해 어느 시간대의 Calendar를 사용하고자 하는지를 파악합니다.

우리나라의 경우 동경시를 사용하고 있습니다.

identifier는 Calendar.Idenifier를 통해 enum type으로 관리되고 있습니다.

<br/> 

```swift
let userAutoUpdateCalendar = Calendar.autoupdatingCurrent
let currentCalendar = Calendar.current
```

객체를 생성자를 통해 만들지 않아도 type variable을 통해 사용자의 달력을 가져올수도 있습니다.

preferred 달력을 계속해서 추적하는 autoupdatingCurrent라는 타입 변수와 현 시점의 달력을 가져오는 current 변수가 존재합니다.

<br/> 

## Calendar Information

Calendar 객체에는 여러 변수와 메서드들이 있고 이들에 접근할 수 있습니다.

- identifier

  캘린더를 구분하는 identifier입니다.

- locale

  지역명을 나타내는 locale 변수입니다.

- timeZone

  시간대를 나타내는 timeZone 변수가 담겨있습니다.

- startOfDay(for: Date) -> Date

  주어진 Date의 가장 처음 시점(0시 0분 0초)를 반환합니다.

<br/> 

## Date 값의 비교

Calendar 객체는 Date 객체에 비해 보다 편리한 날짜 비교 연산을 제공합니다.

```swift
func isDate(Date, equalTo: Date, toGranularity: Calendar.Component) -> Bool
```

이 메서드를 통해 Calendar.Component 단위까지 시간이 같은지를 확인할 수 있습니다.

#### 사용예시

```swift
Calendar.currnet.isDate(Date(),
                        equalTo: Date() + 1000,
                        toGranularity: .minute) // false
```



```swift
func isDate(Date, inSameDayAs: Date) -> Bool
func isDateInToday(Date) -> Bool
func isDateInTomorrow(Date) -> Bool
func isDateInYesterday(Date) -> Bool
func isDateInWeekend(Date) -> Bool
```

위의 메서드들을 통해 날짜가 이번주에 속하는지, 어제에 속하는지, 같은 날에 속하는지 등을

손쉽게 알아낼 수 있습니다.

<br/> 

## Calendar 값의 비교

Date가 부등호를 통해 비교할 수 있던것과는 조금 다르게

Calendar는 `==` 연산과 `!=` 연산만을 지원합니다

<br/> 

## Symbols

Calendar는 여러가지 Symbol들을 제공합니다.

이 Symbol들은 Calendar 객체의 locale 값에 따라 서로 다른 값을 가지고 있습니다.

예를들어 weekdaySymbols는 다음과 같은 값들을 담은 배열입니다.

> ["일요일", "월요일", "화요일", "수요일", "목요일", "금요일", "토요일"]

Symbol은 다음과 같이 분류됩니다.

- Weekday
- Month
- Quarter
- Era

각각의 분류 밑에는 표현을 간소화한 정도에 따른 하위분류가 존재합니다. (long, shortened 등등)

이렇듯 요일, 달, 분기 등을 담은 배열이나 enum을 만들지 않고도 Calendar에서 제공하는 symbol들을 통해 해결할 수 있습니다.

