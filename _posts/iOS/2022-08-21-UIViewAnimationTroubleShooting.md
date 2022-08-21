---
layout: post
title: "UIView animation 트러블슈팅"
categories: [iOS]
tags: 
  - animation
  - UIView
sitemap:
  changefreq: daily
  priority: 1.0
---

## 트러블슈팅 지점

animate 메서드를 통해 이미지를 기존의 위치(A)로부터 새로운 위치(A')으로 이동하는 애니메이션을 구현하려고 했었다.

근데 자꾸 애니메이션 시작 버튼을 누르니까 이미지가 A'위치로 순간이동한 상태에서 A위치로 돌아오는 현상이 일어났다.

대체 왜지? 싶어서 구글링을 통해 예제코드들을 봤는데도 뭐가 다른지 알 수 없었다.



## 해결

스토리보드가 문제였다. (내가 스토리보드가 어떻게 동작하는지를 빠삭하게 모르는게 진짜 문제지만..)

똑같은 뷰를 코드로 구현했을때는 전혀 문제가 없었다.



## 공부할 것 정리

- remove constraint랑 deactivate 차이점 알아보기
- 스토리보드에서 정상적으로 실행 안되던 이유 알아보기 (가능한 선 까지만)
- frame bound transform 차이점과 사용처 알아보기