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

이번 공부의 목표는 Rx 사용시에 컴파일 에러가 떴을때

단순히 예제코드를 구글링으로 찾아서 해결하는게 아닌 왜 컴파일이 안되는지에 대해 이해하는 것이 목적인 공부입니다.



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



### 실행할 클로저를 나중에 전달하기

위의 예제에서는 "해야할 일" 자체는 정해져있던 상황에서 

그 일을 비동기적으로 처리해서 결과값을 나에게 가져다 줘! 였다면

이번에는 "해야할 일" 조차도 정해져있지 않은 상황에서

"해야할 일"은 나중에 알려줄테니 그 "해야할 일"이 들어오면 이 인자를 넣어서 처리해줘~

인 상황입니다.

```swift
class MyObservable<T> {
    private var task: ((@escaping (T) -> Void) -> Void)?

    init(task: @escaping (_ execute: @escaping (T) -> Void) -> Void) {
        self.task = task
    }

    func subscribe(execute: @escaping (T) -> Void) {
        guard let task = task else { return }
        DispatchQueue.global(qos: .background).async {
            task(execute)
        }
    }
}

MyObservable(task: { execute in
	execute("exel file")
})
.subscribe (execute: {str in 
	print(str) 
})
```

**각 기능을 사원, 업무, 엑셀파일로 비유**

- task: 이 객체의 직책 (사원)
- execute: 이 객체의 업무 (엑셀 파일 정리)
- T: 실제로 받게되는 엑셀 파일
- MyObservable의 생성: 일단 엑셀파일을 먼저 주고 좀이따 subscribe를 통해 알려줄 "업무"를 이 엑셀 파일에 적용하면 된다는 사실을 알려줌
  - "일단 엑셀파일을 먼저 준다"는 행위는 뒤에 설명할 operator들에서 더 자연스럽게 읽힘
- subscribe: execute할 업무를 알려주면 이걸 다른 스레드에 가서 실행하고 오라고 지시 (위의 예제에서는 string을 받아서 print하는 것이 그 일)



## RxSwift Operator 구현

위에서 구현한 MyObservable은 Rx의 기본이 되는 틀입니다.

이제 여기에 Rx에서 사용되는 Operator들을 구현해보겠습니다.



### Just

단순히 initialValue가 존재하는 Observable 하나를 반환합니다.

```swift
extension MyObservable {
    static func just(_ value: T) -> MyObservable<T> {
        return MyObservable<T> { execute in
            execute(value)
        } // task
    }
}

MyObservable.just(24)
		.subscribe { number in print(number) }
```

execute라는 클로저를 받아서 거기에 value를 넣어서 실행해줘!



### From

배열을 인자로 받아 배열 내의 원소들을 하나씩 Observable로 바꾸어 반환합니다.

```swift
extension MyObservable {
    static func from(_ value: [T]) -> MyObservable<T> {
        return MyObservable<T> { execute in
            value.forEach { t in
                execute(t)
            }
        } // task
    }
}

MyObservable.from([42, 41, 40])
		.subscribe { i in
		    print(i)
} 
```

execute라는 클로저를 받아서 거기에 [T]의 원소들을 하나씩 넣어가며 실행해줘!



### Map

어떤 collection의 값을 하나하나 꺼내 다른 특정한 type의 값들의 collection으로 만들어줍니다.

```swift
extension MyObservable {
    func map<U>(_ mapper: @escaping (T) -> U) -> MyObservable<U> {
        return MyObservable<U> { execute in
            self.subscribe { t in
                execute(mapper(t))
            }
        } // task
    }
}

// 사용예시
MyObservable.from([42, 41, 40]) // 1
		.map { i in "\(i)" } // 2
		.subscribe { i in // 3
		    print(i) // 4
		} // 5
```

이번에는 직원이 두명입니다.

이해하기 쉽도록 MyObservable\<T\>는 T, MyObservable\<U\>는 U라 생각해봅시다.

T는 mapper라는 클로저를 받습니다.

mapper라는 클로저는 t라는 인자를 받는 함수입니다.

mapper의 결과값을 U의 execute에 넣어주고

그것이 바로 T의 업무가 됩니다. (T의 execute)
