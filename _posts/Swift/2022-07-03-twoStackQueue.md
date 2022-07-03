---
layout: post
title: "Swift two-stack queue 구현"
image:
categories: Swift
tags: 
  - 알고리즘
  - queue
  - two-stack queue
sitemap:
  changefreq: daily
  priority: 1.0
---



오늘은 Swift에서 Queue를 구현하는 방법 중

stack을 2개 활용하는 two-stack queue에 대해 알아봅시다.



Swift의 배열(리스트)는 연결리스트로 구현되어습니다.

Stack의 경우에는 append()와 removeLast()만을 활용하기 때문에 O(1)이 보장되지만 

Queue에서 원소를 pop할때 사용하는 removeFirst()의 시간복잡도는 O(N)입니다.

우리가 Queue에서 기대하는 pop의 시간복잡도는 O(1)이므로 해결방안이 필요합니다.



## Two-stack Queue

```swift
/*
 two-stack queue
 */

struct Queue<T> {
    var enqueue: [T]
    var dequeue: [T]
    var isEmpty: Bool {
        return enqueue.isEmpty && dequeue.isEmpty
    }
    mutating func push(x: T) {
        enqueue.append(x)
    }

    mutating func pop(x: T) -> T? {
        if dequeue.isEmpty {
            while !enqueue.isEmpty {
                dequeue.append(enqueue.removeLast())
            } // dequeue = enqueue.reversed(); enqueue.removeAll()과 같은 작업
        }
        return dequeue.removeLast()
    }
}
```



two-stack queue는 다음과 같이 구성됩니다.

- 원소를 push할때 이를 담을 enqueue
- pop할때 원소를 꺼내올 dequeue



원소를 push할때는 단순히 enqueue의 마지막에 원소를 붙여줍니다. (일반적인 append)

pop할때가 핵심인데 제일 첫 pop연산 시도시에는 dequeue가 비어있으며

enqueue의 첫번째 원소를 pop연산의 결과물로 내보내야 합니다.

이를 위해 dequeue에 enqueue의 원소를 역순으로 채워줍니다.

이때 enqueue에 담겨있는 원소의 양(M)만큼의 연산이 일어나게 됩니다. (O(M))



하지만 이후에 다시 pop을 시도하게 된다면 dequeue의 마지막 원소를 pop해서 내보내면 되므로 O(1)의 시간복잡도로 문제를 해결할 수 있습니다.

dequeue가 비어있을때 이를 채우는 연산의 시간복잡도는 여전히 O(M)이지만 이전에 연결리스트의 removeFirst()만을 활용하여

pop때마다 매번 O(M)의 시간이 들던 것과 비교하면 훨씬 시간이 단축된 것을 볼 수 있습니다.