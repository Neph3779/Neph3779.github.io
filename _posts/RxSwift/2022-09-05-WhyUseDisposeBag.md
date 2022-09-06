---
layout: post
title: "Dispose bag를 사용하는 이유는 뭘까?"
categories: [RxSwift]
tags: 
  - dispose bag
  - RxSwift
sitemap:
  changefreq: daily
  priority: 1.0
---

## 요약

### 1. DisposeBag를 사용하지 않는 경우

```swift
let disposable1 = Observable.just(1)
	.subscribe {
		print($0)
	}

let disposable2 = Observable.just(2)
	.subscribe {
		print($0)
	}

let disposable3 = Observable.just(3)
	.subscribe {
		print($0)
	}


deinit {
	disposable1.dispose()
  disposable2.dispose()
  disposable3.dispose()
}
```



### 2. DisposeBag를 사용하는 경우

```swift
let disposeBag = DisposeBag()

Observable.just(1)
	.subscribe {
		print($0)
	}.disposed(by: disposeBag)

Observable.just(2)
	.subscribe {
		print($0)
	}.disposed(by: disposeBag)

Observable.just(3)
	.subscribe {
		print($0)
	}.disposed(by: disposeBag)
```

disposable들을 담아두었다가 deinit 시점에 dispose 시켜줍니다.

dispose란 옵저버의 구독을 해제함으로써 메모리 정리를 해주는 작업입니다.



1과 2가 같은 동작을 하니 간편한 2번의 방식을 사용하는 것.