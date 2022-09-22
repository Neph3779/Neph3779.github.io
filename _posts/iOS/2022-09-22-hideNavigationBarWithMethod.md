---
layout: post
title: "navigationBar 숨기기에 animation 적용하기"
categories: [iOS]
tags: 
  - navigationBar
  - animation
sitemap:
  changefreq: daily
  priority: 1.0
---

## Navigation Bar 숨기기

기존에는 

```swift
navigationcontroller?.navigationBar.isHidden = true
```

위의 방법으로만 처리해주었었는데

animation 효과를 통해 더 부드러운 UI를 제공할 수있는 메서드가 있었다.

```swift
navigationController?.setNavigationBarHidden(true, animated: true)
```

두 코드로 실행되는 화면을 집중해서 보면 전자에 비해 후자의 방법이 훨씬 부드러운 움직임을 보여준다.



### 전환 화면

| <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220922171711.png" alt="image-20220922171711234" style="zoom:50%;" /> | <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220922171730.png" alt="image-20220922171730104" style="zoom:50%;" /> |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|                         기존의 방법                          |                setNavigationBar 메서드를 이용                |

### 

