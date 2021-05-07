---
layout: post
title: "Frame과 Bounds의 차이"
image:
categories: iOS
tags: 
  - frame
  - bound
  - origin
sitemap:
  changefreq: daily
  priority: 1.0
---

## Frame

Frame은 SuperView의 좌표시스템 안에서의 View의 위치와 크기이다.

frame은 `SuperView의 origin`을 기준으로 움직이며 여기서 origin은 SuperView의 좌상단 꼭짓점이다.

일반적인 좌표계와 달리 원점이 좌상단 꼭짓점이며 x에 대한 양의 방향은 오른쪽, y에 대한 양의 방향은 아래쪽이다.



## Bounds

Bounds는 어떤 View가 지닌 origin의 위치이다.

기본값은 (0,0)이며 Bounds를 조절하면 `SubView의 위치`가 변하게 된다.

이는 SubView의 위치가 SuperView의 origin을 기준으로 결정되기 때문이다.

다시 말해 Bounds를 바꿔주면 SuperView의 위치는 그대로 둔 채로 SubView의 위치만 변경된다.



이는 ScrollView의 핵심이 되는데 ScrollView의 Bounds의 y좌표나 x좌표를 양수로 주게 되면 어떤 일이 일어날지 상상해보자.

ScrollView의 origin을 기준으로 Content가 표시되고 있는 상황에서 origin의 좌표가 바뀌었다면 SubView의 상대적인 위치가 변경되어야 할 것임을 쉽게 유추해볼 수 있다.



## 그림을 통한 이해

그림을 통해 이해를 해보자

그림의 출처는 항상 좋은 글들로 도움을 주시는 `zedd`님 [원문 글 링크](https://zeddios.tistory.com/203)

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210507122232.png" alt="image-20210507122231343" style="zoom:50%;" />

Content View는 저 광활한 그림 전체다.

현재 Content View의 frame은 (0,0)이며

Scroll View의 bounds도 (0,0)이다. (지금 이 문장이 이해가 가지 않는다면 위에 써놓은 설명을 찬찬히 다시 읽어보자)



만약 여기서 Scroll View의 bounds를 (300, 500)으로 바꿔주면 어떤 상황이 될까?

Scroll View의 origin이 (0, 0)에서 (300, 500)으로 변경될 것이고

이로 인해 SuperView의 origin을 기준으로 표시되던 Content View의 위치도 변경될 것이다.



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210507122714.png" alt="image-20210507122713275" style="zoom:50%;" />



바로 이렇게 말이다.



## 추가적으로 생각해볼 것들

- ScrollView의 frame 값을 바꾸면 어떤 일이 일어날까? 
  - ScrollView의 SuperView는 누구일까?
- Content View의 frame 값을 바꾸면 어떤 일이 일어날까?