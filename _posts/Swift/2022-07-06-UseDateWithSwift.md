---
layout: post
title: "Swift로 날짜 다루기 [Date 편]"
image:
categories: Swift
tags: 
  - 날짜 다루기
  - Date
sitemap:
  changefreq: daily
  priority: 1.0
---



오늘은 Swift로 날짜를 다루는 방법들을 정리해보았습니다.

Date, Calendar, DateComponent를 정리할 예정이며

본 글은 Date 객체를 소개하는 글입니다.



## Init

### 기본 Init

```swift
let basicInit = Date()
```

Date 인스턴스를 기본 initializer로 생성하면 현재 시간으로 세팅됩니다.



### timeIntervalSinceNow

```swift
let timeInterValSinceNowInit = Date(timeIntervalSinceNow: 3600)
```

현재로부터의 시간간격을 인자로 주어 그만큼 떨어진 시간을 나타내는 인스턴스가 생성됩니다.

<br/>  

## Type Method, Variable

### 현재 시각 반환

```swift
let now = Date.now
```

단순한 현재 시각을 반환하는 방법입니다. 

Date()와 같은 결과물입니다.



### 지정되지 않은 미래/과거의 값 반환

```swift
let iDontKnowWhenButAnywayItsFutureThanNow = Date.distantFuture
let iDontKnowWhenButAnywayItsPastThanNow = Date.distantPast
```

단순히 현재보다 과거/미래만을 표현하고 싶을때 사용하면 유용한 타입 변수들입니다.

지정되지 않은(알 수 없는) 시점의 값이지만 현재보다 과거/미래라는 사실을 보장합니다.

 <br/> 

## Date의 논리연산

```swift
let itsTrue = Date() > Date.distantPast
let itsFalse = Date() > Date.distantFuture
```

Date를 가지고 논리연산이 가능합니다.

Date값이 더 크다는 것은 더 미래라는 의미입니다.

 <br/> 

## Distance, Interval

```swift
let timeIntervalCanBeNegative = Date().distance(to: Date.distantPast)
let someIntervalFromNow = Date().timeIntervalSince(Date.distantPast)
let someIntervalSinceNow = (Date.distantFuture).timeIntervalSinceNow
```

특정시간으로부터 떨어진 시간 간격을 알려줍니다.

값은 `TimeInterval` type으로 나옵니다.

<br/> 

## Date 값 연산

```swift
var currentDate = Date() // Date는 class가 아닌 struct이기에 var로 선언해야 수정가능
currentDate.addTimeInterval(10000)
currentDate -= 10000
let tomorrow = currentDate.advanced(by: 60*60*24)
let rangeOfDate = currentDate...tomorrow
```

연산기호를 통한 연산, 연산 전용 메서드 두 방법 모두로 연산이 가능합니다.

date의 범위를 변수에 담을 수도 있습니다.

<br/> 

## Date Formatting

### Default Format

```swift
print(Date().formatted())
```

별다른 설정을 하지 않고 formatted() 메서드를 호출하면

date style은 numeric, time style은 shortened로 설정되어 출력됩니다.

### Format Style 지정

```swift
print(Date().formatted(date: .long, time: .standard))
```

위와 같이 직접 date style과 time style을 지정할 수 있습니다.

#### date style의 종류

- abbreviated
- complete
- long
- numeric
- omitted

#### time style의 종류

- complete
- omitted
- shortened
- standard

### Relative Format Style

```swift
let yesterday = currentDate.advanced(by: -60*60*24)
var formatStyle = Date.RelativeFormatStyle() // FormatStyle이란 프로토콜 채택중
formatStyle.presentation = .named
print(yesterday.formatted(formatStyle)) // 어제
formatStyle.presentation = .numeric
print(yesterday.formatted(formatStyle)) // 1일 전
```

1주 전, 1일 전, 어제 등의 관계성 formatting style도 지정할 수 있습니다

### Format Style Custom

```swift
func formatted<F>(_ format: F) -> F.FormatOutput where F : FormatStyle, F.FormatInput == Date
```

formatted 메서드는 위와 같이 생겼습니다.

F가 FormatStyle 프로토콜을 채택하고 FormatInput이라는 associatedType이라면

formatted에 직접 만든 custom format을 적용할 수 있습니다.



## 왜 Date 관련 변수에 1970이란 숫자가 자주 등장하나요?

Unix의 standard time이 1970/1/1 입니다.

Unix -> C -> Swift로 넘어오면서 이것이 이어져온 듯 하네요