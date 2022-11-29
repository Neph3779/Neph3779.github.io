---
layout: post
title: "ScrollView를 터치하면 키보드가 내려가게 하기"
categories: [iOS]
tags: 
  - 
sitemap:
  changefreq: daily
  priority: 1.0
---

## UIScrollView

이번 프로젝트를 진행하다 collectionView의 cell안에 들어간 textView의 editing을 중지하기 위해

textView 바깥 영역을 터치하면 키보드가 내려가도록 하는 로직을 구현하기 위해 공부하던중 

UIScrollView의 프로퍼티에서 그 답을 찾았다.



![image-20221130022128757](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20221130022134.png)



![image-20221130022507908](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20221130022507.png)

onDrag는 scrollView를 드래그하면 키보드가 내려가는 기능이 추가되고

interactive는 "키보드"의 상단부분을 드래그해야 키보드가 내려가는 기능이 추가된다

일반적인 사용자는 text작성 화면 바깥을 "터치"하면 키보드가 내려가도록 하고 싶을 것이라 생각해 onDrag 값을 주어 해결했다.



### 작동화면

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20221130022827.gif" alt="화면 기록 2022-11-30 오전 2.27.41" style="zoom:50%;" />