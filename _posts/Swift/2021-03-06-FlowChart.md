---
layout: post
title: "플로우차트(Flow Chart) 그리는법"
image:
categories: Swift
tags: 
  - Flow Chart
sitemap:
  changefreq: daily
  priority: 1.0
---

## 안녕하세요! Neph🌱입니다!! 😆😆

오늘은 Flow Chart를 그리는 방법에 대해 알아볼거에요~

<br/>

혼자서 작업하는 경우나 단기간동안만 사용할 프로그램을 만들 때는 플로우차트가 굳이 필요하지 않을지도 모릅니다.

하지만 플로우차트로 프로그램 동작과정의 큰 틀을 잡아놓는다면 후에 자신의 코드를 타인이 보게 되는 경우, 혹은 자신이 짠 코드를 다시 보게 될 때 이해하는 속도가 훨씬 빨라질거에요~

<br/>

모든 디테일한 과정을 플로우차트로 표현하겠다는 생각보다는 
#### "플로우차트를 통해 전체적인 코드의 진행과정을 파악하기 쉽게 만드는 것"


이 핵심이지 않을까 생각합니다

<br/>

그럼 이제 실제로 코드를 플로우차트로 바꿔보며 플로우차트 그리는 법을 익혀봅시다!

우선 플로우차트를 그리기 위해 필요한 **도형**들과 그 도형들의 역할에 대해 알아볼게요

<br/>

<p align="center"><img src="/assets/images/posts/BasicShapes.png" alt="BasicShapes" style="zoom:50%;" /></p>

|        도형        |                역할                |
| :----------------: | :--------------------------------: |
| 둥근 모서리 사각형 | 프로세스의 시작과 끝을 알려줍니다. |
|      직사각형      |    기본적인 동작들을 적습니다.     |
|       화살표       | 프로세스의 진행방향을 알려줍니다.  |
|       마름모       |        조건문을 표시합니다.        |
|     평행사변형     |    input과 output을 표시합니다     |

<br/>

---

그럼 이제 이것을 활용해 플로우차트를 그려보도록 하겠습니다!



<br/>

아래의 코드는 흔히들 알고계시는 머지소트(merge sort)를 Swift로 구현한 코드입니다.

<br/>

```swift
func mergeSort(_ left: Int, _ right: Int) {
    let mid = (left + right)/2
    if left < right {
        mergeSort(left, mid)
        mergeSort(mid + 1, right)
        merge(left, right)
    }
}
```

<br/>

여기서 등장하는 `merge`함수는 나누어진 두개의 리스트를 오름차순이 되도록 합쳐주는 역할을 합니다.

(이번 포스팅에서는 `mergeSort`함수만 가지고 `Flow Chart`를 그릴 예정입니다. `merge 함수`는 다루지 않지만 필요하신 분들을 위해 글 제일 밑 부분에 따로 첨부할게요 😁)
<br/>

<details>
<summary style="cursor: pointer; color: #2AC1BC">Merge과정이 이해가 안가신다면 클릭!</summary>
<div markdown="1">
[3,2,8,5,1,4,7,6] 이라는 리스트를 머지소트 시키기 위해선

[3] [2] [8] [5] [1] [4] [7] [6] 과 같이 리스트의 원소 개수의 리스트로 쪼갠 뒤

이를 두 리스트씩 오름차순이 되도록 합쳐주는 작업(Merge)을 해줍니다

[2, 3] [5, 8] [1, 4] [6, 7] 

[2, 3, 5, 8]  [1, 4, 6, 7] 

[1, 2, 3, 4, 5, 6, 7, 8]

이렇게 말이죠!
</div>
</details>
<br/>
자 그럼 이제 `mergeSort` 함수를 가지고 `FlowChart`를 그려보겠습니다.

<p align="center"><img src="/assets/images/posts/MergeSortFlowChart.png" alt="MergeSortFlowChart" style="zoom:50%;" /></p>

코드의 진행과정을 적어보자면

1. 함수가 시작하는 단계를 둥근 모서리 사각형 안에 `mergeSort()`를 넣는 것으로 시작합니다.

2. `parameter`로 `left`, `right`를 받아오므로 (input 값) 평행사변형에 넣어 표시해줍니다.

3. `mid`라는 변수에 (left + right) / 2 를 담아주는 과정은 일반적인 코드의 실행이므로 직사각형에 담아 표시해줍니다.

4. left < right 가 `false`라면 이대로 함수가 종료되고 

   `true`라면 

   - mergeSort(left, mid)
   - mergeSort(mid + 1, right)
   - merge(left, right)

   가 차례로 실행된 뒤 함수가 종료됩니다!


<br/>

오늘은 이렇게 간단하게 플로우차트 그리는 법에 대해 알아보았는데요

현업에서 모든 프로세스를 플로우차트를 통해 해결하는것은 아니지만 특정 부분 혹은 전체적인 틀을 플로우차트를 활용해 만들어놓는다면 유지보수하기가 훨씬 수월해질거에요!

<br/>

더 자세한 도형들의 활용에 대해 알고 싶으시다면 [이 링크](https://www.gliffy.com/blog/guide-to-flowchart-symbols)를 통해 공부해보시면 좋을 것 같습니다!

플로우차트 그리는법은 여기서 마치도록 하겠습니다~ 😆
<br/>
<details>
<summary style="cursor: pointer; color: #2AC1BC">Merge 함수</summary>
<div markdown="1">
```swift
func merge(_ start:Int, _ end: Int) {
    let mid = (start + end)/2

    var lIndex = start
    let lEnd = mid
    
    var rIndex = mid + 1
    let rEnd = end
    
    var sIndex = start
    
    while (lIndex <= lEnd && rIndex <= rEnd) {
        
        if unsorted[lIndex] <= unsorted[rIndex] {
            sorted[sIndex] = unsorted[lIndex]
            lIndex += 1
        } else {
            sorted[sIndex] = unsorted[rIndex]
            rIndex += 1
        }
        sIndex += 1
    }
    
    while (lIndex <= lEnd) {
        sorted[sIndex] = unsorted[lIndex]
        sIndex += 1
        lIndex += 1
    }
    
    for i in start..<sIndex {
        unsorted[i] = sorted[i]
    }
}
</div>
</details>


