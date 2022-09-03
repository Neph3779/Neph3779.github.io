---
layout: post
title: "[작성중] RxSwift 동작 방식 이해하기"
categories: [RxSwift]
tags: 
  - RxSwift
  - Observable
sitemap:
  changefreq: daily
  priority: 1.0
---

## 리액티브 프로그래밍의 발단

다른 스레드에서 진행된 작업의 내용을 return 값으로 받고싶어! (다른 스레드에 보낸 작업은 언제 끝나는지 모름, 이미 메인 스레드는 비동기적으로 다른 작업을 실행중)

위의 생각이 발단이 되어 리액티브 프로그래밍이 만들어졌습니다.



## RxSwift가 동작하는 방식

RxSwift는 Observable로 하여금 무언가를 `subscribe`하게 만들면 

무언가(이벤트를 발생시키는 주체)가 이벤트를 발생시킬 때마다

Observable이 그 이벤트를 받아 처리할 수 있도록 만든 라이브러리입니다.

쉽게 말하면 swift의 didset을 아무데나 가져다 붙여놓을 수 있는 형태로 만들었다고 보시면 됩니다.

(물론 다른 스레드에서 발생한 변경을 캐치합니다.)

이를 이해하기 위해 우선 비동기적으로 값을 가져오는 방법에 대해 살펴보겠습니다.



### 비동기적으로 계산된 값을 사용하기

우선 아래와 같은 작업이 진행된다고 가정해봅시다.

```swift
func veryHeavyMathResult(x: Int) -> Int { ... }
var answer: Int = veryHeavyMathResult(x: 100)
```

answer라는 변수에 함수의 결과값을 저장하는 간단한 코드입니다.

veryHeavyMathResult는 무거운 작업을 하는 함수이니 비동기적으로 값을 가져오도록 하는게 바람직합니다.

값을 비동기적으로 가져온다는 것은 언제 값이 오는지 알 수 없으므로 값이 return 되었을 때 후속작업을 실행해야 합니다.

이를 위해서 `비동기적으로 전달된 결과값을 받아서 사용하는 함수`가 필요하며 이를 보통 `call back`함수 또는 `completion` 함수라고 합니다. (swift에 추가되는 async await를 사용하지 않는다고 가정)

이  내용을 구현한 코드는 아래와 같습니다.

```swift
func someFuncWhoUseAnswer(x: Int, completion: @escaping (Int) -> Void) {
  DispatchQueue.global(qos: backgroud).async {
    var answer = veryHeavyMathResult(x: x)
    completion(answer)
	}
}

someFuncWhoUseAnswer(completion: { answer in
  	// use answer here
})
```



이번에는 비동기적으로 어떤 클로저를 받아와서 이를 비동기적으로 실행하는 코드를 살펴보겠습니다.

```swift
class MyObservable<T> {
    private var task: ((@escaping (T) -> Void) -> Void)?

    init(task: @escaping (_ closureForDetermineT: @escaping (T) -> Void) -> Void) {
        self.task = task
    }

    func subscribe(_ closureToExecute: @escaping (T) -> Void) {
        guard let task = task else { return }
        DispatchQueue.global(qos: .background).async {
            task(closureToExecute)
        }
    }
}

MyObservable(task: { parameterClosureForTask in // closureForDetermineT는 task의 T를 확정짓기 위해 사용됨
    parameterClosureForTask("parameter type for closure to execute; String")
    // 위의 문장 자체가 closureForDetermineT라는 클로저
    // parameterClosureForTask라는 클로저에 String을 인자로 받음으로써
    // Generic type인 T를 확정지어주는 역할을 함
})
.subscribe({str in print(str)})
```



코드가 조금 복잡합니다. 차근차근 뜯어봅십다.

우선 핵심이 되는 subscribe 메서드를 보면 closureToExecute라는 클로저를 받아와서 이를 비동기적으로 실행합니다.

> 그럼 그냥 받아온 closureToExecute를 바로 실행하면 안되나요?

아쉽게도 그건 불가능합니다. 

closureToExecute의 인자는 제네릭 타입인 T인데 closureToExecute를 DispatchQueue 블록에서 실행하려면 이 T에 구체적인 타입이 있는 인자값을 넣어줘야 합니다. 

이러한 상황을 해결하기 위해 task라는 "T라는 인자를 받는 클로저"를 인자로 받는 클로저가 존재합니다.

task의 경우에는 "T라는 인자를 받는 클로저"를 인자로 넣어주면 되므로 closureToExecute를 넣어주면 실행이 가능합니다.

~~꽤나 tricky 하네요~~



아래의 코드는 위의 코드를 더 추상화해본 버전입니다. 

init 시점에 "T type의 값을 인자로 받는 클로저"를 인자로 전달하던 것에서

T type의 값만 전달할 수 있도록 바꾸어 보았습니다.

```swift
class MyObservable<T> {
    private var task: ((@escaping (T) -> Void) -> Void)?

    init(initialValue: T) {
        self.task = { closure in
            closure(initialValue)
        }
    }

    init(task: @escaping (_ closureForDetermineT: @escaping (T) -> Void) -> Void) {
        self.task = task
    }

    func subscribe(_ closureToExecute: @escaping (T) -> Void) {
        guard let task = task else { return }
        DispatchQueue.global(qos: .background).async {
            task(closureToExecute)
        }
    }
}

let myObservable = MyObservable<String>(initialValue: "hello")
myObservable.subscribe({ str in
    print(str)
})
```



## RxSwift Operator 구현

위에서 구현한 MyObservable은 Rx의 기본이 되는 틀입니다.

이제 여기에 Rx에서 사용되는 Operator들을 구현해보겠습니다.



### Just

단순히 initialValue가 존재하는 Observable 하나를 반환합니다.

```swift
extension MyObservable {
    static func just(_ value: T) -> MyObservable<T> {
        return MyObservable<T>(initialValue: T)
    }
}

MyObservable.just(24)
		.subscribe { number in print(number) }
```



### From

배열을 인자로 받아 배열 내의 원소들을 하나씩 Observable로 바꾸어 반환합니다.

```swift
extension MyObservable {
    static func from(_ value: [T]) -> MyObservable<T> {
        return MyObservable<T> { receiver in
            value.forEach { t in
                receiver(t)
            }
        }
    }
}

// 사용 예시
MyObservable.from([42, 41, 40])
		.subscribe { i in
		    print(i)
} 

// 반환하는 MyObservable<T>의 task에 해당하는 블록
{ receiver in
		value.forEach { t in
		receiver(t)
	}
}

// MyObservable<T>의 task의 인자값(task에 넣어줄 파라미터 클로저)
value.forEach { t in
	receiver(t)
}

// subscribe시에 task의 인자((T)->Void)로 들어갈 블록 = receiver
// 사용 예시에서 receiver에 해당하는 블록
{ i in
	print(i)
}
```

위의 코드에서 사용예시 이전에 결정된 것들

- MyObservable\<T\>의 task 

위의 코드에서 사용예시를 작성함으로써 결정되는 것들

- MyObservable\<T\>의 task의 인자값 (task에 넣어줄 파라미터 클로저)
  - receiver에 해당

- T의 구체적 type

Operator from은 receiver라는 클로저를 받아 value(배열)의 원소들을 하나씩 receiver 클로저의 인자로 전달해주면서 실행시켜주는 타입 메서드입니다.



### Map

어떤 collection의 값을 하나하나 꺼내 다른 특정한 type의 값들의 collection으로 만들어줍니다.

```swift
extension MyObservable {
    func map<U>(_ mapper: @escaping (T) -> U) -> MyObservable<U> {
        return MyObservable<U> { receiver in
            self.subscribe { t in
                receiver(mapper(t))
            }
        }
    }
}

// 사용예시
MyObservable.from([42, 41, 40]) // MyObservable<T>의 task가 결정됨
		.map { i in "\(i)" } // MyObservable<U>를 반환
		.subscribe { i in // 반환된 MyObservable<U>를 사용하는 예시
		    print(i)
		}

// 반환할 MyObservable<U>의 task에 해당하는 블록
{ receiver in
	self.subscribe { t in // 이때 self는 Observable<T> (이 타입메서드를 호출한 인스턴스)
		receiver(mapper(t))
	}
}

// Observable<T>의 subscribe 메서드의 인자로 들어갈 블록
// 즉 Observable<T>의 task의 인자값으로 들어갈 블록
{ t in
	receiver(mapper(t))
}

// mapper에 해당하는 블록
{ i in 
 "\(i)" 
}
```

위의 코드에서 사용예시 이전에 결정된 것들

- MyObservable\<U\>의 task
- MyObservable\<T\>의 task의 인자값

위의 코드에서 사용예시를 작성함으로써 결정된 것들

- MyObservable\<T\>의 task
- MyObservable\<U>의 task의 인자값
