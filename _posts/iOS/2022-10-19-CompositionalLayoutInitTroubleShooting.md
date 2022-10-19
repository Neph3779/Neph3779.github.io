---
layout: post
title: "Compositional Layout 트러블슈팅"
categories: [iOS]
tags: 
  - 
sitemap:
  changefreq: daily
  priority: 1.0
---

## Compositional Layout의 Init 종류

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20221019163727.png" alt="image-20221019163727040" style="zoom:67%;" />



첫번째와 두번째 init의 설명을 보면 single section을 사용하는 경우에 사용 가능하다고 적혀있다.

따라서 section이 여러개인 경우에 첫번째 init을 사용하게 되면 에러가 난다.

<br/> 

여러개의 section이 존재하지만, 모든 section의 내용이 똑같은 경우에도 (SectionProvider의 sectionIndex 값을 활용해 분기하는 로직이 없더라도) 세번째와 네번째의 initializer를 통해서 초기화를 해주어야 에러가 나지 않는다.



