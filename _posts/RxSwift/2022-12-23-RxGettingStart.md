---
layout: post
title: "RxSwift Getting Started 정리"
categories: [RxSwift]
tags: 
  - 
sitemap:
  changefreq: daily
  priority: 1.0
---

본 글은 RxSwift의 공식문서 [Getting Started](https://github.com/ReactiveX/RxSwift/blob/main/Documentation/GettingStarted.md)를 정리한 글입니다.

<br/> 

## Observable

- `Observable`은 `Sequence와` 동등
- `ObservableType.subscribe` 메서드는 `Sequence.makeIterator` 메서드와 동등
- itertator의 next() 메서드를 호출하는 대신 ObservableType.subscribe 메서드에 값을 전달하여 sequence의 element를 받아 사용할 수 있도록 함

**암묵적으로 Observable이 보장하는 것**

- 어떤 쓰레드에서 element를 produce하든간에 **하나의 Event 처리가 끝나기 전에** **다른 Event의 처리가 시작되는 경우는 없다**



**Event, Observable, ObservableType의 내부 구현**

```swift
enum Event<Element>  {
    case next(Element)      // next element of a sequence
    case error(Swift.Error) // sequence failed with error
    case completed          // sequence terminated successfully
}

class Observable<Element> {
    func subscribe(_ observer: Observer<Element>) -> Disposable
}

protocol ObserverType {
    func on(_ event: Event<Element>)
}
```



- Event는 `next`, `error`, `completed로` 구성됨
- sequence element를 free 시키기 위해, 반환된 subscription(Disposable)을 dispose 시킴



## Disposing

- dispose()를 직접 호출하지 않는 것을 권장
- 보통의 경우에는 DisposeBag에 담아서 한꺼번에 처리
- dispose된 시점 이후에 subscribe 클로저에 명시된 동작이 실행되는 경우가 있는가?
  - 만약 동일한, serial 스케쥴러에서 dispose 시켜주었다면 불가능
  - 이 외의 경우에는 가능
- element를 produce하는 동작과 subscription을 dispose하는 동작은 병렬적으로 발생할 수 있음
- dispose 메서드가 return된 이후에는 어떠한 동작도 수행되지 않는 것이 보장됨



## Dispose Bag

- DisposeBag이 deallocate되면 bag에 추가된 disposable들을 각각 dispose시킴
- DisposeBag 자체는 dispose 메서드를 호출할 수 없는데, 만약 immediate하게 dispose 작업이 필요하다면 기존의 disposeBag에 새로운 DisposeBag 인스턴스를 만들어 주면 됨



## 나만의 Observable 만들기

- observable을 **단순히 만든다고 이것이 어떠한 작업을 진행하는 것은 아님**
- 단순히 Observable을 리턴하는건 어떠한 부수작용(side effect)도 일으키지 않음 
- Observable은 그저 **어떻게 sequence를 생성할 것인지**와 이때 **어떠한 인자를 사용할 것인가**만 정의함
- subscribe 메서드가 불렸을때에서야 비로소 Sequence generation에 들어감

[**예제**] rx의 operator 중 하나인 just의 구현

```swift
func myJust<E>(_ element: E) -> Observable<E> {
    return Observable.create { observer in
        observer.on(.next(element))
        observer.on(.completed)
        return Disposables.create()
    }
}

myJust(0)
    .subscribe(onNext: { n in
      print(n)
    })
```

**create 함수**

- 클로저를 통해 subscribe 메서드를 구현하기 편리하도록 만들어진 함수
- subscribe 메서드처럼 `하나의 인자`와 `observer`를 필요로하며 **disposable을 반환**함
- 이런식으로 구현된 Sequence는 동기적으로 동작함
  - element generation이 이루어지고
  - element generation은 subscribe call이 disposable을 return하기 전에 종료됨
  - 그렇기 때문에 어떤 disposable을 return하든 아무 상관이 없음 (element generating 프로세스가 방해받지 않음)



## 일하는 Observable 만들기

[**예제**] 특정 시간 간격마다 next 이벤트를 발생시키는 Interval operator

```swift
func myInterval(_ interval: DispatchTimeInterval) -> Observable<Int> {
    return Observable.create { observer in
        print("Subscribed")
        let timer = DispatchSource.makeTimerSource(queue: DispatchQueue.global())
        timer.schedule(deadline: DispatchTime.now() + interval, repeating: interval)

        let cancel = Disposables.create {
            print("Disposed")
            timer.cancel()
        }

        var next = 0
        timer.setEventHandler {
            if cancel.isDisposed {
                return
            }
            observer.on(.next(next))
            next += 1
        }
        timer.resume()

        return cancel
    }
}
```

[**사용 예시**]

```swift
let counter = myInterval(.milliseconds(100))

print("Started ----")

let subscription1 = counter
    .subscribe(onNext: { n in
        print("First \(n)")
    })

let subscription2 = counter
    .subscribe(onNext: { n in
        print("Second \(n)")
    })

Thread.sleep(forTimeInterval: 0.5)

subscription1.dispose()

print("Disposed")

Thread.sleep(forTimeInterval: 0.5)

subscription2.dispose()

print("Disposed")

print("Ended ----")
```

[**print 결과**]

```swift
Started ----
Subscribed
Subscribed
First 0
Second 0
First 1
Second 1
First 2
Second 2
First 3
Second 3
First 4
Second 4
Disposed
Second 5
Second 6
Second 7
Second 8
Second 9
Disposed
Ended ----
```

- 각 구독마다 분리된 sequence를 만들어줌
- Operator는 기본적으로 stateless



## share operator를 통한 subscription의 공유

- 여러개의 observer가 하나의 subscription을 통해 event 전달을 공유받도록 하고 싶은 경우에 사용
- 그 전에 결정해야 할 두가지 사항
  - 누군가가 나중에 구독하는 시나리오에서 이전에 방출했던 event들을 뒤늦게 구독한 구독자에게도 전달할 것인가? 전달한다면 몇개의 과거 이벤트를 전달할 것인가
  - 공유된 이벤트를 언제 실행할지에 대한 결정 (refCount를 사용하거나 직접 알고리즘을 제작하여 사용할수도 있음)
- 가장 흔하게 사용되는 조합은 `replay(1).refCount()`, aka `share(replay: 1)`



[**예제**] Interval에 share(replay: 1)을 붙이고 두 개의 subscribe를 실행하는 경우

```swift
let counter = myInterval(.milliseconds(100))
    .share(replay: 1)

print("Started ----")

let subscription1 = counter
    .subscribe(onNext: { n in
        print("First \(n)")
    })
let subscription2 = counter
    .subscribe(onNext: { n in
        print("Second \(n)")
    })

Thread.sleep(forTimeInterval: 0.5)

subscription1.dispose()

Thread.sleep(forTimeInterval: 0.5)

subscription2.dispose()

print("Ended ----")
```



[**print 결과**]

```swift
Started ----
Subscribed
First 0
Second 0
First 1
Second 1
First 2
Second 2
First 3
Second 3
First 4
Second 4
Second 5
Second 6
Second 7
Second 8
Second 9
Disposed
Ended ----
```

- 하나의 subscription을 공유하는 것이기 때문에 MyInterval의 Observable create 시점에 출력되는 "Subscribed"가 한번만 출력된 것을 확인할 수 있다.
  - 즉 subscription2는 subscribe시에 별도의 subscription을 만들지 않고 기존의 것을 공유받음

[**예제**] HTTP request가 Rx 내부에 래핑되어있는 형태 (Interval operator의 생성과 유사)

```swift
extension Reactive where Base: URLSession {
    public func response(request: URLRequest) -> Observable<(response: HTTPURLResponse, data: Data)> {
        return Observable.create { observer in
            let task = self.base.dataTask(with: request) { (data, response, error) in

                guard let response = response, let data = data else {
                    observer.on(.error(error ?? RxCocoaURLError.unknown))
                    return
                }

                guard let httpResponse = response as? HTTPURLResponse else {
                    observer.on(.error(RxCocoaURLError.nonHTTPResponse(response: response)))
                    return
                }

                observer.on(.next((httpResponse, data)))
                observer.on(.completed)
            }

            task.resume()

            return Disposables.create {
                task.cancel()
            }
        }
    }
}
```

- Disposable을 create할때 task를 cancel시켜주는 로직까지 포함되어있는 것을 확인할 수 있다.



## Operators

- https://reactivex.io/에 다양한 operator들에 대한 설명이 있음
- 어떤 operator를 쓸지 잘 모르겠다면 https://reactivex.io/documentation/operators.html#tree를 참고



## Infallible

- fail하거나 error를 emit하지 않는 Observable이라 생각하면 됨
- 따라서 Infallible을 만드는 경우에 error를 emit할 수 없음
- 절대 실패하지 않거나 MainScheduler에서 사용하고 싶지 않거나 share()를 암시적으로 사용하고 싶은 경우에 Infallible을 사용 
  - Driver, Signal이 대표적인 사용예시



## Error handling

### Observables의 비동기적 에러 핸들링 메커니즘

- 하나의 sequence가 error를 내뿜고 종료되면 나머지 관련된 sequence들도 모두 error와 함께 종료됨
- catch operator를 통해 recover가 가능함
- retry를 통해 error가 발생한 sequence를 다시 시도하는 것도 가능

### Hooks와 디폴트 에러 핸들링

- RxSwift는 개발자가 onError를 구현하지 않은 경우를 위해 global Hook을 제공함
- 클로저를 만들어 Hooks.defaultErrorHandler에 할당하면 system 내의 unhandled error를 처리할 수 있음
- 기본적으로 Hooks.defaultErrorHandler는 단순히 DEBUG모드에서 받은 에러를 print해줌, RELEASE모드에선 아무 동작도 하지 않음
- 디테일한 logging을 원하면 Hooks.recordCallStackOnError 플래그를 true로 두면 됨
  - 이는 기본적으로 DEBUG모드라면 Thread.callStacksSymbols를 반환할 것이고
  - RELEASE 모드에서는 empty stack trace가 이루어질거임
  - Hooks.customCaptureSubscriptionCallStack을 override해서 커스터마이징 구현도 가능함

## Debugging Compile Errors

- RxSwift/RxCocoa 코드를 깔끔하게 적다보면 compiler의 타입 추론에 심하게 의존하게 됨
- 이는 간헐적으로 compiler error를 야기하는 요인이 됨
- 이럴때는 클로저를 축약하지 말고 인자의 type, 반환값의 type을 명시해주면 해결할 수 있음



## Debugging

- 디버거만 써도 유용하겠지만 debug operator와 함께 쓰면 더 효과적
- debug operator는 모든 이벤트를 standard output에 맞춰 print 해줌
  - label을 달아주는 것도 가능

[**예제**]

```swift
let subscription = myInterval(.milliseconds(100))
    .debug("my probe")
    .map { e in
        return "This is simply \(e)"
    }
    .subscribe(onNext: { n in
        print(n)
    })

Thread.sleepForTimeInterval(0.5)

subscription.dispose()
```

[**출력**]

```
[my probe] subscribed
Subscribed
[my probe] -> Event next(Box(0))
This is simply 0
[my probe] -> Event next(Box(1))
This is simply 1
[my probe] -> Event next(Box(2))
This is simply 2
[my probe] -> Event next(Box(3))
This is simply 3
[my probe] -> Event next(Box(4))
This is simply 4
[my probe] dispose
Disposed
```



### Enabling Debug Mode

- 앞서 언급된 Debugging, HTTP request logging을 위해선 Debug 모드를 켜줘야 함
- TRACE_RESOURCES 플래그를 RxSwift target build settings와 다른 Swift Flag 아래에 추가해줘야 함
- 관련된 내용은 https://github.com/ReactiveX/RxSwift/issues/378를 참고하면 도움이 될 것임



### Debugging memory leaks

- 디버그 모드에서 Rx는 모든 할당된 리소스들을 Resources.total이라는 글로벌 변수에서 track함
- 메모리 leak을 검사하고 싶다면 RxSwift.Resources.total을 print함으로써 확인할 수 있음



효과적인 메모리 leak 검사방법

- navigate to your screen and use it
- navigate back
- observe initial resource count
- navigate second time to your screen and use it
- navigate back
- observe final resource count

참고사항

- initial 시점과 final 시점의 resources counts가 다르다면 메모리 누수가 발생하였다는 신호

- 2번 navigation 해보도록 권장하는 것은 첫번째 navigation 시점에 lazy 로딩 때문에 메모리 누수를 감지하지 못할 위험을 없애기 위함임



## UI layer 팁

### Threading

- Observable은 MainScheduler에 value를 보내야 함
- API를 MainScheduler에서 return되도록 하는 것을 권장
- 백그라운드 쓰레드에서 UI binding 작업을 하려하면 RxCocoa의 Debug build에서 에러를 던져줄거임
- URLSession extension은 MainScheduler에서 return하는 것이 디폴트 설정이 아니니 유의



### Errors

- UIKit 요소에 failure한 것을 bind할 수 없음
- Observable이 fail할지 안할지 알 수 없다면 catchErrorJustReturn(valueThatIsReturnedWhenErrorHappens) 메서드를 활용해 에러가 발생하더라도 밑에 있는 sequence들은 여전히 동작하도록 할 수 있음
- retry를 활용해서도 해결할 수 있음



### Sharing subscription

- UI layer에서 subscription을 공유하고 싶은 경우가 많음
- 여러개의 UI 요소에 대해 각각 HTTP call을 보내고 싶지는 않을것

```swift
let searchResults = searchText
    .throttle(.milliseconds(300), scheduler: MainScheduler.instance)
    .distinctUntilChanged()
    .flatMapLatest { query in
        API.getSearchResults(query)
            .retry(3)
            .startWith([]) // clears results on new search term
            .catchErrorJustReturn([])
    }
    .share(replay: 1)    // <- notice the `share` operator
```

- 한번만 계산, 결과를 공유
- share를 변환 가장 마지막에 붙이는건 좋은 습관임
- Driver를 택하는 것도 고려해볼 사항
  - main UI Thread에서의 동작을 보장
  - error가 발생하지 않음



## Making HTTP requests

[**예제**]

```swift
URLSession.shared.rx.response(myURLRequest)
    .debug("my request") // this will print out information to console
    .flatMap { (data: NSData, response: URLResponse) -> Observable<String> in
        if let response = response as? HTTPURLResponse {
            if 200 ..< 300 ~= response.statusCode {
                return just(transform(data))
            }
            else {
                return Observable.error(yourNSError)
            }
        }
        else {
            rxFatalError("response = nil")
            return Observable.error(yourNSError)
        }
    }
    .subscribe { event in
        print(event) // if error happened, this will also print out error to console
    }
```



### Logging HTTP traffic

- RxCococa는 HTTP request info를 debug 모드에서 기본적으로 log함
- URLSession.rx.shouldLogRequest 클로저를 overwrtie하여 logging할 것과 logging 하지 않을 것에 대한 분기처리가 가능함

```swift
URLSession.rx.shouldLogRequest = { request in
    // Only log requests to reactivex.org     
    return request.url?.host == "reactivex.org" || request.url?.host == "www.reactivex.org"
}
```

