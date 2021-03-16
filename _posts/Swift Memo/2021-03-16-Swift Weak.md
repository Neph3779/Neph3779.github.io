---
layout: post
title: "Swift Weak"
image:
categories: [Swift Memo]
tags: 
  - weak
  - strong
  - unowned
sitemap:
  changefreq: daily
  priority: 1.0
---

### 강한참조 vs 약한참조

ARC는 컴파일 시점에 retain release와 같은 메모리 관리 메소드를 자동으로 삽입해줌. 가비지 컬렉터와 달리 참조사이클을 자동으로 처리하지 않기 때문에 객체에 강한 참조가 남아있다면 해당 객체는 메모리 해제가 되지 않음



따라서 서로 다른 객체가 서로를 강하게 참조하면 순환참조가 발생해 메모리 누수(Leak)가 발생

이걸 방지하기 위해서 약한 참조를 사용

weak과 unowned는 참조하는 객체에 대해 참조계수를 증가시키지 않으므로 순환참조 방지 가능



weak vs unowned



weak는 참조하는 객체의 참조계수 0되면 nil로 대체됨

unowned는 참조값 그대로 유지



그럼 unowned는 왜 쓰는거지??

아직 모르겠구만



-> unowned는 값이 있음을 가정하고 쓰는 non-optional이구나!

