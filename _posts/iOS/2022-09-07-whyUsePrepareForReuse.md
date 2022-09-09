---
layout: post
title: "prepareForReuse는 왜 쓰는걸까?"
categories: [iOS]
tags: 
  - cell
  - tableView
  - collectionView
  - prepareForReuse
sitemap:
  changefreq: daily
  priority: 1.0
---

## cellForRowAt v.s. prepareForReuse

우선 prepareForReuse 메서드의 설명을 보면

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220907154631.png" alt="image-20220907154631124" style="zoom:50%;" />

prepareForReuse는 dequeueReusableCell(withIdentifier)이 return 되기 직전에 호출되는 메서드입니다.

퍼포먼스 저하를 막기 위해서 cell의 content와 관련없는 작업들(ex. alpha값 수정, selection 상태 조절 등)을 진행할 것을 요구하고 있습니다.

cellForRowAt에서는 반드시 모든 content를 cell 재활용을 위해 초기화할 것을 요구하고 있네요.

추가로 reuse identifier가 없는 경우나 reconfigureRows(at: ) 메서드를 통해 기존의 cell을 업데이트 한 경우에는 prepareForReuse가 호출되지 않는다고 합니다.



## 요약

내용 변경은 cellForRowAt에서

색상, 알파값 등 content 이외의 것들은 퍼포먼스 저하를 막기 위해 prepareForReuse에서