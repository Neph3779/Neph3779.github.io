---
layout: post
title: "iOS 원티드 프리온보딩 회고"
categories: [회고]
tags: 
  - 
sitemap:
  changefreq: daily
  priority: 1.0
---

## 회고

TIL을 다시 쓰려 시도했었지만 정말 배운걸 정리하려면 글로 정리해야 한다 생각이 될 뿐더러

TIL을 쓰는 공간에 그 날 하루에 대한 회고만 남는듯 하여 회고 카테고리를 만들었다.

<br/> 

## 프리온보딩

야곰 아카데미에서 원티드와 함께 프리온보딩이란 것을 진행했다

3개의 과제를 3주동안 1주에 하나씩 진행하는 프로그램이었는데

한 주라는 기간에 비해서 과제의 난이도가 아주 낮지는 않았다.

1주차:  https://github.com/Neph3779/ios-wanted-GyroData

2주차: https://github.com/Neph3779/ios-wanted-BoxOffice

3주차: https://github.com/Neph3779/ios-wanted-PersonalScheduler

참여기업과의 채용연계가 진행된다고는 하는데,

산업기능요원으로 취직해야하는 상황도 그렇고, 참여기업들이 모두 iOS직군을 뽑고 있는것도 아니기에 큰 의미는 없어보였다.

<br/> 

이번 3주동안의 소득은 크게 3가지 정도인 것 같다.

1. 이력서 특강을 통해 이력서를 쓸때 중요한 점을 알게되었다.
2. Yapp(동아리)에서 만들고 있는 프로젝트의 예행연습을 해보았다. (run time dynamic cell height, clean architecture의 coordinator & DIConatiner 사용법 등)
3. 파이어베이스를 좀 다양하게 써봤다. (OAuth, FireStore(DB), FireStorage(파일 저장소))

<br/> 

누가 채점을 하는것도 아니고 목표한 기업의 과제도 아닌데도

그냥 하다보니까 욕심이 생겨서 꽤 많이 열심히 코드를 짜봤다. (아무래도 1, 2주차는 팀원이 붙어있는 팀 과제라 대충하기엔 곤란한 상황이었던 것 같다.)

Diffable DataSource의 사용에도 어느정도 익숙해졌고 (개인적으론 디퍼블이 Compostional Layout보다 더 공부할 부분이 많은듯? (Cell Registration 관리 등))

파이어베이스도 개인 프로젝트에서는 애용하게 될 것 같다. (db를 만드는게 이렇게 간단하다니!... 빌드가 너무 느려지는건 어쩔 수 없지만..)

이력서 특강도 생각했던것보다 훨씬 유익했는데, 유데미와 콜라보해서 진행한 이력서 포폴 특강보다 10배 이상은 유익했다.

특강을 한줄로 정리하면 "지원하는 그 기업한테 잘보일만한 내용을 담은 이력서로 매번 커스터마이징해라"였던 것 같다.

부트캠프 얘기도 쓰지 말라고 하던데.. 뭔가 부트캠프 얘기를 안쓰고 독학한것처럼 쓰면 거짓말 하는 기분이랄까..

어차피 코드 스타터 리뷰어, 서포터즈 활동 내역도 적어야하니.. 적당히 너무 드러내지 않는 선에서 써야겠다.



## 딴소리

그나저나 https://github.com/ReactiveX/RxSwift/pull/2471

이건 대체 언제 머지해주련지.. 읽어주긴 하는건지.. 

공식 가이드에 있는 오타(프린트 결과물 오류)라 수정이 좀 되었으면 하는데 많이 바쁘신가보다.

<br/> 

https://developer.apple.com/documentation/uikit/views_and_controls/collection_views/implementing_modern_collection_views

이 애플 예재앱도 Item Badges 셀 누르니까 크래시가 터지던데

제보하는 곳을 찾아서 얘도 제보해야겠다.