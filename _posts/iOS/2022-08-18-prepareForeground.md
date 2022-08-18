---
layout: post
title: "[번역, 작성중] Preparing Your UI to Run in the Foreground"
categories: [iOS]
tags: 
  - foreground
  - app life cycle
sitemap:
  changefreq: daily
  priority: 1.0
---

## 개요

앱의 UI가 화면에 띄워지는 것을 준비하기 위해 foreground 전환을 사용합니다. foreground로의 상태변환은 일반적으로 유저의 액션에 대한 응답입니다. 예를 들어 유저가 앱의 아이콘을 터치하면 시스템은 앱을 실행하고 foreground 상태로 만듭니다. 앱의 UI 업데이트, 리소스 요청, 유저의 요청을 핸들링하는 서비스의 시작을 위해 foreground로의 상태변환을 사용할 수 있습니다.

모든 상태 전환시에는 UIKit이 알맞은 delegate 객체에 notification을 보내게 됩니다. (appDelegate, sceneDelegate)

