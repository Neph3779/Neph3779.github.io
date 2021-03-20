---
layout: post
title: "Swift Weak"
image:
categories: [Swift Memo]
tags: 
  - weak
  - strong
  - unowned
sitemap:
  changefreq: daily
  priority: 1.0
---

### 강한참조 vs 약한참조

ARC는 컴파일 시점에 retain release와 같은 메모리 관리 메소드를 자동으로 삽입해줌. 가비지 컬렉터와 달리 참조사이클을 자동으로 처리하지 않기 때문에 객체에 강한 참조가 남아있다면 해당 객체는 메모리 해제가 되지 않음



따라서 서로 다른 객체가 서로를 강하게 참조하면 순환참조가 발생해 메모리 누수(Leak)가 발생

이걸 방지하기 위해서 약한 참조를 사용

weak과 unowned는 참조하는 객체에 대해 참조계수를 증가시키지 않으므로 순환참조 방지 가능



weak vs unowned



weak는 참조하는 객체의 참조계수 0되면 nil로 대체됨

unowned는 참조값 그대로 유지



그럼 unowned는 왜 쓰는거지??

아직 모르겠구만



-> unowned는 값이 있음을 가정하고 쓰는 non-optional이구나!



### Weak 과 strong 무슨 차이이며 왜 @IBOutlet의 디폴트는 weak 일까?

Swift에선 Reference Count (이하 RC)가 0이 되면 메모리를 해제 시켜줍니다. Automatic reference counting(ARC)이라는 훌륭한 자동 메모리 관리를 통해 메모리 관련 문제를 대부분 해결해줍니다.

하지만 예외의 상황도 있는 법..

이런 상황에선 메모리 누수가 일어나게 됩니다.

```swift
class TestClass{
    var testClass: TestClass? = nil
    init(){
        print("init")
    }
    deinit(){
        print("deinit")
    }
}

var testClass1: TestClass? = TestClass()
var testClass2: TestClass? = TestClass()

//시점 1
testClass1?.testClass = testClass2
testClass2?.testClass = testClass1

//시점 2
testClass1 = nil
testClass2 = nil
```

시점 1의 상황 

![img](https://t1.daumcdn.net/cfile/tistory/99A8253359E0502225)

시점 2의 상황

![img](https://t1.daumcdn.net/cfile/tistory/9932F63359E0504A2B)

보이는 것처럼 각각의 instance가 내부적으로 서로를 `강한 참조`하고 있기 때문에 RC값은 0이 아닙니다. 따라서 우리가 저 instance들을 메모리로부터 없앨 방법이 없습니다. (Retain Cycle이 돌게 됨)



이러한 불상사를 방지하기 위해 `weak`을 사용합니다. `weak`을 사용한 약한 참조는 RC값을 증가시키지 않기 때문에 만약 `TestClass`의 testClass 프로퍼티가 약한 참조로 선언되었다면 아래와 같은 상황이 됩니다.

![img](https://t1.daumcdn.net/cfile/tistory/9931B63359E0505D35)

weak 참조를 한 경우엔 instance의 메모리가 해제되면 자동으로 nil이 됩니다. 여기서 알아두어야 할 점은 weak 참조 변수는 반드시 optional이어야 한다는 점입니다. (nil이 들어갈 것이기 때문)

`unowned`참조를 사용한다면 instance가 메모리에서 해제된 이후에 해당 메모리 영역이 nil이 아니라 그대로 남아있게 됩니다. 하지만 이 경우에는 해제된 메모리 영역을 접근하지 않는다는 보장이 있어야합니다! optional을 사용하지 않아 메모리 측면에서 이득이 있을 수 있겠으나.. 안정성을 위해선 weak을 택하는 것이 좋아보입니다.



[Retain Cycle 정리글 (이동건)](https://baked-corn.tistory.com/30)

[Retain Cycle 예시용 App](https://github.com/godrm/RetainCycleApp)

[그 외 도움 되는 링크](https://github.com/godrm/SICP-Swift)

