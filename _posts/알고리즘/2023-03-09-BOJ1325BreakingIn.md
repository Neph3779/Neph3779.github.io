---
layout: post
title: "[Swift] 백준 1325 효율적인 해킹 풀이"
image:
categories: [알고리즘]
tags: 
  - Algorithm
  - 2023 Kako
sitemap:
  changefreq: daily
  priority: 1.0
---

[문제링크](https://www.acmicpc.net/problem/1325)

dfs bfs dp 브루트포스등 다양한 문제 풀이가 존재하는 문제였다.

swift로 절대 못풀겠다는 글들이 많이 보이던데 메모리 제한도 적고 5초라는 널널해보이는 시간제한에 비해 시간초과를 맞기 너무 쉬운 문제라 그런 것 같다.

노드의 수가 10,000보다 작으므로 UInt16을 사용해 메모리 초과를 피했고

직접 만든 two stack queue를 이용해 bfs로 문제를 풀어낼 수 있었다.

꼼꼼히 봐야했던 지점은 코드의 46번째 줄의 visited 처리와 48번째줄의 visited 체크이다.

46번째 줄은 cycle이 생길 수 있는 그래프를 다루기 때문에 자기자신을 방문처리 하지 않았을 때 문제가 생길 수 있으니 유의해야 하며, 48번째 줄은 child의 child로 자기자신(현 시점 parent)를 가질 수 있으니 visited 체크를 꼭 해주어야 했다.

> child가 parent를 신뢰한다는 의미에서 child와 parent라는 용어를 사용하였다.



```swift
import Foundation

struct Queue<T> {
    var pushStack = [T]()
    var popStack = [T]()
    var isEmpty: Bool {
        return pushStack.isEmpty && popStack.isEmpty
    }
    var count: Int {
        return pushStack.count + popStack.count
    }

    mutating func enqueue(_ element: T) {
        pushStack.append(element)
    }

    mutating func dequeue() -> T? {
        if popStack.isEmpty {
            while let popped = pushStack.popLast() {
                popStack.append(popped)
            }
        }
        return popStack.popLast()
    }
}

func answer() {
    let nm = readLine()!.split(separator: " ").map { Int($0)! }
    let n = nm[0]
    let m = nm[1]
    var childs = [[UInt16]]()
    for _ in 0..<n+1 {
        childs.append([])
    }
    var maxHackCount = -1
    var canHackCount: [UInt16] = Array(repeating: 0, count: n+1)

    for _ in 0..<m {
        let temp = readLine()!.split(separator: " ").map { Int($0)! }
        childs[temp[1]].append(UInt16(temp[0]))
    }

    for i in 1..<n+1 {
        var visited = Array(repeating: false, count: n+1)
        var queue = Queue<UInt16>()
        visited[i] = true
        for child in childs[i] {
            if !visited[Int(child)] {
                queue.enqueue(UInt16(child))
                canHackCount[i] += 1
                visited[Int(child)] = true
            }
        }
        while let dequeued = queue.dequeue() {
            for child in childs[Int(dequeued)] {
                if !visited[Int(child)] {
                    queue.enqueue(child)
                    canHackCount[i] += 1
                    visited[Int(child)] = true
                }
            }
        }
        maxHackCount = max(maxHackCount, Int(canHackCount[i]))
    }

    for i in 0..<canHackCount.count where canHackCount[i] == maxHackCount {
        print("\(i)", terminator: " ")
    }
}

answer()

```



평소에는 백준 문제 풀이를 하나하나 블로그에 올리지 않지만, 빠른 입출력, 매우 최적화된 queue 없이도 충분히 swift로 풀 수 있는 문제인데 인터넷에 해답이 잘 안올라와있는 듯 싶어 포스팅..

