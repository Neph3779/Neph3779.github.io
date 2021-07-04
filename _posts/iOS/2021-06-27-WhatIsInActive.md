---
layout: post
title: "inActive 상태란 무엇일까?"
image:
categories: iOS
tags: 
  - app life cycle
sitemap:
  changefreq: daily
  priority: 1.0
---

## inActive 상태란?

앱의 생명주기는 크게 아래와 같이 나뉜다.

1. not running (unattached)
2. foreground
   1. inActive
   2. active
3. background
4. Suspended



not running은 말 그대로 앱이 실행중이지 않은 상태이며

앱 실행시 foreground 상태로 진입한다 (background 상태에서도 진입 가능)

이 때 active 상태로 가기 전 inActive상태를 거치게 된다.

Suspended 상태는 메모리의 공간확보 문제로 iOS가 임의로 메모리에서 해제시키는 상태를 말한다.

이 때 앱은 not running 상태로 돌아간다. 









run loop를 돌고 있으면 active

그냥 아무것도 안하고 있으면 inactive

background로 가는 경우가 여러가지 있음 (실행되던 앱이 ...)

이런 경우들이 바로 

status bar 내리거나

전화오거나.. interrupt, launch screen 에서 inactive가 될 수 있다.



화면 끄면 inActive 거쳐서 background로 감

app switcher상태일때는 inActive