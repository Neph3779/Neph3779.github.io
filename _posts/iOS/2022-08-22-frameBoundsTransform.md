---
layout: post
title: "frame, bounds, transform 정리 요약"
categories: [iOS]
tags: 
  - frame
  - bounds
  - transform
sitemap:
  changefreq: daily
  priority: 1.0
---

## Frame

정의: superview의 좌표계를 기준으로 설정되는 view의 위치

- 좌표의 원점은 좌상단 꼭지점
- superview의 bounds의 (0, 0)부터 떨어진 위치가 origin
- view가 rotate되었다면 rotate된 뷰를 감싸는 minimum한 직사각형이 해당 view의 frame이 됨
  <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220822194215.jpeg" alt="post-thumbnail"  />
  즉, View B의 frame은 원점이 (140, 65)이고 width height가 (320, 320)이다.
- center의 경우 frame의 중앙지점이다.



## Bounds

정의: 자신의 좌표계를 기준으로 설정되는 view의 위치

- bounds를 변경한다는 것은 subview의 frame의 원점 위치, subView가 볼 수 있는 화면의 크기 등이 변경됨
- 만약 superView의 frame의 크기보다 subView의 원점의 위치가 더 크다면 화면에서 벗어나 보이지 않게 되는 것
- superview의 bounds의 x, y 값을 각각 100씩 늘려줬다고 가정하면
  - subview는 superview의 (0, 0)부터 자신의 frame의 origin만큼 떨어진 위치에 view를 그려야 하므로
  - subview는 상대적으로 왼쪽 위로 100씩 이동하는 것처럼 보인다.



| <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220822195623.png" alt="image-20220822195616814" style="zoom:50%;" /> | <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220822195641.png" alt="image-20220822195635645" style="zoom:50%;" /> | <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220822195707.png" alt="image-20220822195701674" style="zoom:50%;" /> |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 원래 상태                                                    | bounds의 x, y를 각각 100씩 증가                              | bounds의 width를 100 감소                                    |



## Transform

- 뷰의 크기를 조절하거나 회전할 때 사용
- 오토레이아웃은 untransformed frame을 기준으로 뷰의 정렬을 계산함

transform은 글을 따로 작성해야 할 분량이므로 이번 글에서는 다루기 어려울 것 같습니다.

 [Frame과 Bounds 이해하기 (velog.io)](https://velog.io/@baecheese/Frame과-Bounds-이해하기) 해당 블로그 글 링크를 참조해주세요