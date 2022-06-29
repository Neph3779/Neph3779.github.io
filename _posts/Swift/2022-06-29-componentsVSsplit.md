---
layout: post
title: "Swift split과 components 차이"
image:
categories: Swift
tags: 
  - split
  - components
sitemap:
  changefreq: daily
  priority: 1.0

---



오늘은 문자열을 분할할때 사용하는 split과 components의 차이에 대해 알아보았습니다.



#### 요약

여러개의 원소들을 가지고 분할하고 싶을때는  components

하나의 원소만을 기준으로 분할하고 싶을때는 split



## split

split의 계층은 다음과 같습니다.

![image-20220629205907781](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220629205907.png)



- components와 다르게 Swift 자체에 포함되어있기에 Foundation을 import 해주지 않아도 사용할 수 있습니다.
- String의 인스턴스 메서드입니다.



![image-20220629210129356](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220629210129.png)



split은 3개의 parameter를 받는 메서드입니다.

seperator 파라미터에는 어떤 원소를 기준으로 분할할 것인지, 그 원소를 적어줍니다.

maxSplits를 통해 분할로 나오는 원소의 개수를 제한할 수 있습니다.

omittingEmptySubsequences를 통해 만약 빈 항목이 있다면 그것을 결과 배열에 넣어줄지 말지의 여부를 결정할 수 있습니다.

가령 예를 들자면 "hello swift "라는 마지막에 " "이 존재하는 문장을 " "를 통해 분할한다고 하면

"hello", "swift", ""라는 세개의 원소로 분할할 수 있습니다.

이때 마지막 원소 ""를 결과 배열에 포함시킬지의 여부를 결정합니다.



## components

components의 계층은 다음과 같습니다.

![image-20220629211009163](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220629211009.png)



components는 Foundation 프레임워크에 포함된 NSString의 인스턴스 메서드입니다.

![image-20220629211055796](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220629211055.png)



split과 다르게 separator라는 하나의 파라미터만을 받습니다.

이때 여러개의 원소를 배열에 담아 넣어줄 수 있는데

가령 "10-30/29*39+1" 과 같은 식에서 연산기호를 제외한 채 숫자만을 담은 배열을 원한다면

`str.components(seperateBy: ["+", "-", "*", "/"])` 와 같이 작성해주면 됩니다.

[10, -, 30, /, 29 ...]와 같이 숫자와 기호를 동시에 담은 배열을 원한다면 따로 작업을 해주어야 합니다.