---
layout: post
title: "UITabbar Bar Tint Color 변경 트러블슈팅"
categories: [iOS]
tags: 
  - tabBar
  - barTintColor
sitemap:
  changefreq: daily
  priority: 1.0
---

## 문제

1. tabBar.barTintColor를 통해서 색상을 지정해주었지만 bar의 색이 바뀌지 않음
2. tabBar의 불투명도 설정을 isTrasnlucent값 변경을 통해 시도해보았지만 실패 (tabBar가 기존처럼 살짝 불투명한 상태를 원했는데 isTranslucent 값 변경으로는 해결되지 않았음)

### 기존 코드

```swift
tabBar.barTintColor = .blue
tabBar.isTranslucent = true
```

barTintColor를 변경해보았지만 적용되지 않았고

isTranslucent를 true로 주어서 불투명한 tabBar를 띄워주려 했지만 이 또한 변경되지 않았음



### 해결한 코드

```swift
private func setUpTabbarAppearance() {
        tabBar.standardAppearance = UITabBarAppearance().then {
            $0.configureWithOpaqueBackground()
            $0.backgroundColor = .systemBackground
        }
        tabBar.scrollEdgeAppearance = tabBar.standardAppearance
        tabBar.tintColor = .red
    }
```



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220905101300.png" alt="image-20220905101253959" style="zoom:50%;" />

barTintColor의 공식문서를 열어 계층도를 보면

위와 같이 barTintColor의 직접 변경이 Legacy Customization의 카테고리에 속해있음을 알 수 있다.

iOS 13 이후에서는 standardAppearance 프로퍼티를 통해 tabBar의 appearance 설정을 하기를 권장하고 있다.

<br/> 

불투명도의 경우 appearance의 isTranslucent를 변경해주거나 configureWithOpaqueBackground 메서드를 사용하면 됐다.

barTintcolor은 appearance의 backgroundColor를 변경해주면 됐다.

아쉬운점은 tabBar의 tinitColor은 원래의 방식대로 (마지막 줄처럼) 설정해줄 수밖에 없었던 부분