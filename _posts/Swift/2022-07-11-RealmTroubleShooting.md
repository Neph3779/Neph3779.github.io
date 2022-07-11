---
layout: post
title: "Realm 트러블슈팅 (Realm accessed from incorrect thread)"
image:
categories: Swift
tags: 
  - Realm
  - Thread
sitemap:
  changefreq: daily
  priority: 1.0
---



## 트러블 발생 지점

![](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220711155613.png)

65번째 줄에서 Realm에 접근하였고

이 줄의 실행까지는 문제가 없는데

66번째 줄부터 시작하는 main thread의 UI 업데이트 작업에서 오류가 발생했다.

이유는 Realm accessed from incorrect thread인데 의문인 것은

이 블록에서는 Realm에 접근하지 않는다는것.. (products라는 인스턴스가 보유한 내부 배열을 가지고 collectionView를 채웠기 때문에 Realm에는 다시 접근하지 않는 중)



문제 해결시에 다시 해결방법 및 문제였던 부분 작성예정
