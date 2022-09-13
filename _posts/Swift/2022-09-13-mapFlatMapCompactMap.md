---
layout: post
title: "Map, CompactMap, FlatMap"
categories: [Swift]
tags: 
  - map
  - compactMap
  - flatMap
sitemap:
  changefreq: daily
  priority: 1.0
---

## Map, CompactMap, FlatMap

### Map

![image-20220913184204453](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220913184241.png)

transform이라는 `(Self.Element) throws -> T` 타입의 클로저를 인자로 받아 Sequence의 원소들을 차례대로 transform의 인자로 주어 나오는 결과값을 다시 배열로 묶어 반환해주는 인스턴스 메서드

"복잡도"는 `O(n)` 입니다. (공식문서상에 시간복잡도인지 공간복잡도인지 정확히 명시되어있지 않습니다. 이 경우에는 시간복잡도라고 보는게 맞겠습니다.

<br/> 

### CompactMap

![image-20220913184222619](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220913184247.png)

Map처럼 클로저를 인자로 받아 Sequence의 원소들을 차례대로 클로저에 대입하는 것은 Map과 같으나, 만약 클로저 실행의 결과값이 nil인 경우 Map은 nil을 배열의 원소로 삽입하지만 CompactMap은 nil값은 배열에 삽입하지 않는다는 차이가 있음

```swift
let possibleNumbers = ["1", "2", "three", "///4///", "5"]

let mapped: [Int?] = possibleNumbers.map { str in Int(str) }
// [1, 2, nil, nil, 5]

let compactMapped: [Int] = possibleNumbers.compactMap { str in Int(str) }
// [1, 2, 5]
```

복잡도가 Map과 다른데, O(n+m)입니다. (n은 주어진 배열의 길이, m은 결과로 나오는 배열의 길이)

아마 공간복잡도 측면에서 차이가 발생한 것 같습니다.

<br/>

### FlatMap

![image-20220913184300169](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220913184300.png)

Map, CompactMap과 동일하게 클로저를 인자로 받아 기존 배열의 원소들을 차례대로 넣어가며 실행합니다. 

위의 두 메서드와는 다르게 transform 클로저의 반환 타입이 Sequence를 만족하는  `SegmentOfResult`입니다.

즉 클로저에서 "배열"의 일부를 반환하며 반환된 배열들을 모두 합쳐 새로운 하나의 1차원 배열로 만들어줍니다.

FlatMap도 복잡도는 O(n+m)입니다.

예전에는 flatMap으로도 nil을 걸러내는 작업을 했었지만 현재는 compactMap의 등장으로 기능이 분리되었습니다.