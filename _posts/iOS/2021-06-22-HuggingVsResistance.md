---
layout: post
title: "[Auto Layout] Hugging vs Resistance"
image:
categories: 
tags: 
  - 
sitemap:
  changefreq: daily
  priority: 1.0
---

## Hugging vs Resistance

hugging은 말 그대로 안고있으려는 힘이다. 조금 덧붙이자면 더이상 늘어나지 않으려는 힘을 hugging이라고 한다. stackView의 distrbution 속성이 fill일 경우 내부의 요소들은 stackView 안에서 멋대로 늘어난다. 이 때 hugging priority가 높다면 늘어나지 않고 intrinsic size를 유지하려하고, 낮다면 늘어난다.



resistance는 눌리지 않으려는 힘이다. 화면에 담을 수 없는 크기의 요소들을 우겨넣으면 어떤 요소는 필연적으로 줄어들거나 가려져야한다. 이 때 resistance priority가 낮은 요소부터 줄어들거나 가려지기 시작한다.

