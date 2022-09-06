---
layout: post
title: "특정 corner에만 radius 주기"
categories: [iOS]
tags: 
  - corner radius
  - corner radius only top
sitemap:
  changefreq: daily
  priority: 1.0
---

## 방법

```swift
$0.layer.cornerRadius = 20
$0.layer.maskedCorners = [.layerMinXMinYCorner, .layerMaxXMinYCorner]
```

layer의 maskedCorners 배열을 원하는 코너에만 적용시켜주면 된다. (예시의 경우 좌상단, 우상단)

stackView를 썼는데 corner radius를 주고 싶은 경우에 아주 유용하게 사용할 수 있다.

(stackView 자체에 corner radius를 주는 방법보다 훨씬 쉽고 간편)



## MaskedCorners가 뭘까?

우선 익숙한 clipToBounds에 대해서 정리하자면

View가 있고 그 내부 요소가 있을때

내부요소가 View에서 삐져나온 부분이 있다면 그 부분을 잘라내서 보이지 않게 해주는 속성이다.



clipToBounds와 maskToBounds는 기능상 동일하므로 이를 바탕으로 이해해보자면

(단, clipToBounds는 UIView의 속성, maskToBounds는 CALayer의 속성이라는 차이점이 존재)

특정 corner를 mask 할지의 여부를 결정하는 것이 maskedCorners라고 할 수 있다.