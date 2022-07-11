---
layout: post
title: "Realm 트러블슈팅 (Realm accessed from incorrect thread)"
image:
categories: Swift
tags: 
  - Realm
  - Thread
sitemap:
  changefreq: daily
  priority: 1.0
---



## 트러블 발생 지점

![](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220711155613.png)

65번째 줄에서 Realm에 접근하였고

이 줄의 실행까지는 문제가 없는데

66번째 줄부터 시작하는 main thread의 UI 업데이트 작업에서 오류가 발생했다.

이유는 Realm accessed from incorrect thread인데 의문인 것은

이 블록에서는 Realm에 접근하지 않는다는것.. (products라는 인스턴스가 보유한 내부 배열을 가지고 collectionView를 채웠기 때문에 Realm에는 다시 접근하지 않는 중)



~~문제 해결시에 다시 해결방법 및 문제였던 부분 작성예정~~



## 문제해결

가장 핵심은

"같은 큐에 넣는다고 같은 쓰레드에 보내는 것이 아니다!" 였다.

내가 맞닥뜨린 오류는 "서로 다른 쓰레드에서 같은 자원(Realm DB)에 접근했습니다."라는 오류였는데

이전에 잘못 생각한 부분은 rank update를 맡긴 큐도 global 큐이고

products를 현재 자료에 맞게 업데이트 하는 부분도 global 큐에 보내서 했으니까 ok 아닌가? 였다.

위에도 적었듯 같은 큐(글로벌 큐)에 넣는다고 해결되는 것이 아닌 같은 `쓰레드`에 넣어야 했는데

이건 같은 큐에 뒤이어 넣는다고 되는 것이 아니다.

<br/> 

65번째 줄이 정상적으로 넘어가고 66번째 줄 실행시에 오류가 나던 것은

reloadData때문이 아닌 Realm가 뒤늦게 DB로의 중복접근을 캐치하고 오류를 뿜어주었기 때문에

reloadData를 하면 Realm 오류가 나는 것처럼 보였던 것이었다.

<br/> 

오랜만에 다시 GCD, global Queue등에 대해 복습해볼 수 있는 좋은 기회가 되었다.
