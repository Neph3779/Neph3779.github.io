---
layout: post
title: "Swift 딕셔너리 값이 존재하는지 확인하는 법"
image:
categories: Swift
tags: 
  - Dictionary
sitemap:
  changefreq: daily
  priority: 1.0
---

## Dictionary에 특정 Key값이 존재하는지 확인하는 법

```swift
if myDictionary["key"] != nil {
	//...
}
```

메서드가 하나쯤 있을법 한데, 특별히 메서드를 지원하지는 않는다.

그냥 키값을 넣었을때 nil이 나오면 값이 없다고 판단할 수 있다.

가끔씩 이상하게 생각이 안나서 글 하나 업로드..

