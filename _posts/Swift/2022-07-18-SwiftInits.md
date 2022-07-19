---
layout: post
title: "[작성중] Swift initializer 총정리"
image:
categories: Swift
tags: 
  - init
  - required init
  - convinience init
sitemap:
  changefreq: daily
  priority: 1.0

---

이번 글에서는 [Swift.org 문서]([Initialization — The Swift Programming Language (Swift 5.7)](https://docs.swift.org/swift-book/LanguageGuide/Initialization.html))를 바탕으로 Swift에서 사용되는 다양한 initializer의 종류와 그 사용처에 대해 알아보겠습니다.



## Initializers

- Deinit은 class type의 경우에만 구현할 수 있습니다.
- 클래스와 구조체는 인스턴스 생성 시점에 반드시 모든 저장 프로퍼티에 값을 저장해야합니다.
- optional type의 값은 값 지정이 이루어지지 않는다면 nil값으로 초기화됩니다.
- 초기값은 init을 통해서 할당할수도 있고 정의부에서 default 값을 할당할수도 있습니다. (이때 전자와 후자의 경우 모두 프로퍼티 옵저버는 동작하지 않습니다.)

### Default value

만약 항상 같은 값으로 프로퍼티를 초기화해야 한다면 initializer를 통해서가 아닌 default value를 통해서 지정하길 권고하고 있습니다. 이 방법이 보다 명확하고 default initializer와 initializer inheritance의 이점을 챙길 수 있습니다.

### Customizing Init

init은 파라미터의 이름으로 구분됩니다. 

받는 파라미터의 수, 각각의 자료형이 모두 같더라도 파라미터의 이름을 다르게 해준다면 서로 다른 initializer로 동작합니다.

### default init, memberwise init

init을 구현하지 않아도 모든 값에 default 값이 있으면 default init을 사용할 수 있습니다.

구조체의 경우 default 값이 없어도 memberwise init을 통해 값을 지정하며 초기화할 수 있습니다.

class에 memberwise init이 없는 이유는 상속 때문입니다. (어떤 클래스를 상속하여 거기에 저장 프로퍼티를 추가한다면? memberwise init을 통해 초기화되어야 했던 여러 프로퍼티들이 초기화되지 않으면서 문제 발생 가능)



## Initializer Delegation

initializer는 또 다른 initializer를 호출할 수 있으며, 이를 통해 중복코드를 줄이거나 간결성을 높일 수 있습니다.

initializer delegation은 값타입과 클래스타입(참조타입)이 서로 다르게 동작합니다.

구조체같은 값타입은 상속을 지원하지 않고 자기 자신의 initializer만 호출할 수 있기 때문에 과정이 단순합니다.

(상속받은 부모 클래스의 initializer를 호출하는 등의 행위가 없음)

하지만 클래스 타입의 경우 상속이 가능하기 때문에 모든 저장 프로퍼티가 initialize 시점에 초기화되는 것을 보장해야합니다.

구조체에서 `self.init()`과 같이 적어 다른 init을 호출할 수 있습니다. 이때 initializer 안에서만 init 호출이 가능합니다.



#### 활용 예시

```swift
struct Rect {
    var origin = Point()
    var size = Size()
    init() {}
    init(origin: Point, size: Size) {
        self.origin = origin
        self.size = size
    }
    init(center: Point, size: Size) {
        let originX = center.x - (size.width / 2)
        let originY = center.y - (size.height / 2)
        self.init(origin: Point(x: originX, y: originY), size: size)
    }
}
```





## Class Inheritance and Initialization

부모 클래스로부터 상속받은 저장 프로퍼티를 포함한 모든 저장 프로퍼티는 initialization 과정에서 값이 할당되어야 합니다.



### Initializer Delegation 규칙 (클래스 타입)

1. Designated init은 반드시 직전 부모 클래스의 designated init을 호출해야 한다. (자신이 가장 부모인 클래스면 예외)
2. convenience init은 반드시 같은 클래스의 init을 호출해야 한다.
3. convenience init은 최종적으로 반드시 designated init을 호출해야 한다.

<img src="https://docs.swift.org/swift-book/_images/initializerDelegation02_2x.png" alt="initializerDelegation02_2x" style="zoom:50%;" />



위의 그림과 같이 호출이 진행되며, 이러한 계층 구조를 띄고있기에 designated init을 "funnel" 포인트라고 합니다. (funnel: 굴뚝, 깔때기)



## Two-Phase Initialization

클래스의 초기화는 두 단계로 이루어집니다.

1단계에서는 각 저장 프로퍼티에 초기 값이 할당됩니다.

모든 저장 프로퍼티의 초기 상태가 결정되었다면, 두번째 단계로 진입합니다.

두번째 단계에서는 인스턴스가 "준비"상태로 간주되기 전에 저장 프로퍼티를 customize할 수 있는 기회가 주어집니다.



two-phase 초기화는 초기화 과정을 안전하게 해줌과 동시에 클래스 계층에 있는 각 클래스에게 완전한 유연성을 제공합니다.

two-phase 초기화는 저장 프로퍼티가 초기화되기 전에 접근되는 것을 막을 수 있고, 또 다른 initializer로 인해 저장 프로퍼티에 의도와 다른 값이 세팅되는 것을 방지할 수 있습니다.



### compiler가 진행하는 Safety check

1. Designated init은 반드시 모든 저장 프로퍼티를 super class의 init을 호출하기 전에 초기화 해야 합니다.
   (object의 메모리 공간은 모든 저장 프로퍼티가 초기화된 상태에서 딱 한번만 측정되기 때문에)
2. Designated init은 상속받은 저장 프로퍼티의 값을 변경하기 전에 super class의 init을 호출해야 합니다. 만약 이 순서대로 하지 않으면 원하는 값을 designated init에서 할당하더라도 super class의 init이 뒤에 호출되므로 그 값이 다시 변경될 수 있습니다.
3. 2번과 마찬가지의 이유로 convenience init은 어떤 값을 저장 프로퍼티에 할당하기 전에 designated init을 호출해야 합니다.
4. initializer는 1단계(two-phase 중 첫 단계)가 끝나기 전에 인스턴스 메서드를 호출할 수 없고, 인스턴스 프로퍼티의 값을 읽을 수  없습니다. 



### Two phase 진행 과정

#### Phase 1

<img src="https://docs.swift.org/swift-book/_images/twoPhaseInitialization01_2x.png" alt="../_images/twoPhaseInitialization01_2x.png" style="zoom:50%;" />

- designated/convenience init이 클래스에서 호출됩니다.
- 클래스의 새 인스턴스를 위한 메모리 공간이 할당됩니다. (아직 메모리 공간이 초기화된 것은 아닙니다.)
- Designated initializer가 모든 저장 프로퍼티가 값을 가진 것을 확인하고 (해당 클래스의 것만, 부모 클래스의 것은 신경 X) 저장 프로퍼티의 메모리 공간을 initialize 합니다.
- Designated initializer가 상위 클래스에게 hands off 하고 상위 클래스의 init을 호출하여 같은 작업을 진행합니다.
- 이 작업은 체인의 top에 도달할때까지 진행됩니다.
- top 체인까지 도달했다면 마지막 클래스(최상위 부모 클래스)는 모든 저장 프로퍼티에 값이 들어간 것을 확인합니다. 인스턴스의 메모리 초기화 작업이 끝났다면 phase 1이 끝난 것입니다.



#### Phase 2

<img src="https://docs.swift.org/swift-book/_images/twoPhaseInitialization02_2x.png" alt="../_images/twoPhaseInitialization02_2x.png" style="zoom:50%;" />

- 체인을 다시 타고 내려오면서 각 designated initializer는 인스턴스를 customize할 수 있는 기회를 부여받습니다. 이제 인스턴스는 self에 접근할 수 있으며 인스턴스 메서드를 호출할수도 있습니다.
- 최종적으로 convenience init이 customize의 기회를 부여받습니다. 이때 convenience init도 self를 사용할 수 있습니다. (당연하게도 convenience init을 통해 init하지 않았다면 이 과정은 생략됩니다.)



## Initializer 상속과 오버라이딩

Swift는 상위클래스의 initializer를 default로 상속받지 않습니다. 이는 상위 클래스로부터 받은 간략한 init이 사용되는 것을 막습니다. (subclass가 복잡한 처리과정을 통해 init되어야 하는데 이를 위해 만든 init이 아닌 상위클래스로부터 상속받은 init을 사용해버리면 의도와 다른 무의미한 instance가 생성될 가능성이 있고, 이를 방지하는 차원)

