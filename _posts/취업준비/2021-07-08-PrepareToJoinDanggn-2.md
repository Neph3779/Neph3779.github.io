---
layout: post
title: "[#2] 당근마켓 화면 분석-1"
image:
categories: 취업준비
tags: 
  - 취업준비
  - 당근마켓 인턴
sitemap:
  changefreq: daily
  priority: 1.0
---

## 당근마켓 클론코딩을 목표로 하는 화면

### "내 근처" 탭

| <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210708192923.jpeg" alt="IMG_6FB09D3842B8-1" style="zoom:50%;" /> | <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210708193047.jpeg" alt="IMG_00AAE9892BE6-1" style="zoom:50%;" /> | <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210708193101.jpeg" alt="IMG_711A55BD70C1-1" style="zoom:50%;" /> |
| :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|                              1                               |                              2                               |                              3                               |



## 화면의 구성 및 특징

### 기본구성

- UITabBarController를 활용하여 5개의 탭을 사용하고 있다.

- UINavigationController가 UITabBarController 바로 위에 위치하고 있다.
- Seach Bar를 포함하고 있는 부분(header view)과 아래의 부분(collection view)이 명확히 구분되어있다.



### 새로고침 화면

| <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210708215402.jpeg" alt="IMG_D4988E27B7B6-1" style="zoom:50%;" /> | <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210708215504.jpeg" alt="IMG_EBFDD6618D93-1" style="zoom:50%;" /> |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|                       새로고침 화면 1                        |                       새로고침 화면 2                        |



화면을 아래로 당기면 `새로고침`이 실행된다.

 ~~이 때 화면을 잡아당기는 정도에 따라 `주황색 로딩 인디케이터`가 서서히 채워지는데 이 로딩 인디케이터가 모두 채워질 정도로 잡아당겨야 비로소 새로고침이 된다.~~ 

~~(화면 1에서는 아직 로딩 인디케이터가 다 채워지지 않아서 저 상태에서 손을 떼면 새로고침이 이루어지지 않고 화면 2처럼 끝까지 당겨야 새로고침이 이루어진다.)~~

로딩 인디케이터가 채워지는 것과 관계없이 아주 빠르게 드래그를 하는 경우에도 새로고침이 이루어진다.



#### 새로고침이 되지 않는 경우

| <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210708215745.jpeg" alt="IMG_888B81AE5527-1" style="zoom:50%;" /> |
| :----------------------------------------------------------: |
|          새로고침이 정상적으로 이루어지지 않는 모습          |

어떨 때에는 화면을 밑으로 당겨도 새로고침이 제대로 이루어지지 않는다. 어떤 조건에 의해 새로고침이 이루어지는지 파악이 필요해보인다.



###  로딩 화면

| <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210708193305.jpeg" alt="IMG_6AFE4DD1C6F6-1" style="zoom:33%;" /> |
| :----------------------------------------------------------: |
|                          로딩 화면                           |

로딩시에는 placeholder를 통해 화면에 나타날 것들의 위치를 미리 표시해준다.

특이한 점은 collection view의 첫번째 section으로 추정되는 위치에는 저렇게 placeholder가 들어있지만 두번째 section부터의 placeholder는 나타나지 않는다.



### NavigationBar

| 1    | ![image-20210708221803474](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210708221822.png) |
| ---- | ------------------------------------------------------------ |
| 2    | ![image-20210708221909760](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210709001155.png) |



`그림 1`은 기본 navigation bar의 모습이다.

collection view의 두번째 section이 표시되는 순간부터 `그림 2`에서 보이는 것처럼 돋보기 모양 아이콘이 추가된다.

| <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210709001210.png" alt="image-20210708222019028" style="zoom:50%;" /> |
| :----------------------------------------------------------: |
|                          검색 화면                           |

검색 화면은 다음과 같이 구성되어있다. 최근 검색한 내용이 캐시데이터로 저장되는 모양이고, 추천 검색어는 더이상 좌우 스크롤을 통해 확인하는 형태가 아닌 하나의 section에 스크롤 없이 담겨있는 모습이다.