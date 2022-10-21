---
layout: post
title: "URL Components 트러블 슈팅"
categories: [iOS]
tags: 
  - 
sitemap:
  changefreq: daily
  priority: 1.0
---

## URL Components

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20221021101137.png" alt="image-20221021101137671" style="zoom:67%;" />

URL Components의 url 연산프로퍼티의 설명을 보면

path는 "/"로 시작하거나 빈 스트링이여야 한다고 나와있다

시작할때 "/"로 시작해주지 않아서 에러가 생겼었다.