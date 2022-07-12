---
layout: post
title: "BFS 몇번에 걸쳐 이동했는지 count하는법"
image:
categories: Swift
tags: 
  - BFS
sitemap:
  changefreq: daily
  priority: 1.0

---

## BFS 코드

```swift
func answer() {
    var dayCount = 0
    while true {
        for _ in 0..<queue.count {
            guard let next = queue.pop() else {
                return
            }
            addNeighbors(x: next.j, y: next.i)
        }

        if queue.isEmpty { break }
        dayCount += 1
    }

    for i in 0..<Y {
        for j in 0..<X {
            if matrix[i][j] == 0 {
                dayCount = -1
                break
            }
        }
    }

    print(dayCount)
}
```



위의 코드는 [백준 7576번 토마토 문제](https://www.acmicpc.net/problem/7576)의 정답 코드 중 일부이다.

[정답코드 링크](https://github.com/Neph3779/SwiftBOJ/blob/main/Class3/7576.swift)



가장 핵심은 4번째 줄에서 while의 검사문으로 `!queue.isEmpty` 가 아닌

for문을 사용해서 **현재 큐에 들어있는 원소의 수만큼만** 반복해주는 것이다.



종종 쓸 일이 있는데 가끔 방법이 생각이 안날때가 있어서 블로그에 작성했다.