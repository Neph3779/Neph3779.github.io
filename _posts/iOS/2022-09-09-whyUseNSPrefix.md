---
layout: post
title: "NS 접두사는 왜 붙이는걸까?"
categories: [iOS]
tags: 
  - NS
  - NextStep
sitemap:
  changefreq: daily
  priority: 1.0
---

## NS

NS는 Next Step의 약자입니다.

Next Step이란 회사의 코드를 애플이 사용하게 되면서 해당 코드들도 딸려온 것이죠.

근데 애플이 지금까지도 NS 키워드를 붙일 이유가 있을까요? 

이유는 Objective-C가 C의 extension이기 때문에 C++에서처럼 namespace를 사용할 수 없습니다.

그렇기 때문에 unique한 prefix symbol을 붙여서 서로간의 간섭을 막고자 한 것이죠.

framework 단위로 무언가를 만들고 싶다면 이러한 unique prefix를 반드시 사용하여야 한다고 합니다.

자세한 내용은 [ChooseYourOwnPrefix - CocoaDev](https://cocoadev.github.io/ChooseYourOwnPrefix/) 에 서술되어 있습니다.

<br/> 

괜히 NS, CG 등이 접두사로 붙던게 아니었다는걸 알게되었습니다.

괜히 궁금했던 내용이라 포스트!