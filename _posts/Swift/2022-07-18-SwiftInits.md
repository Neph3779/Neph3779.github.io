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



### Initializers

Deinit은 class type의 경우에만 구현할 수 있습니다.

클래스와 구조체는 인스턴스 생성 시점에 반드시 모든 저장 프로퍼티에 값을 저장해야합니다.

optional type의 값은 값 지정이 이루어지지 않는다면 nil값으로 초기화됩니다.

init을 통해서 저장할수도 있고 정의부에서 default 값을 지정해줄수도 있습니다. (이때 전자와 후자의 경우 모두 프로퍼티 옵저버는 동작하지 않습니다.)

### Default value

만약 항상 같은 값으로 프로퍼티를 초기화해야 한다면 initializer를 통해서가 아닌 default value를 통해서 지정하길 권고하고 있습니다. 보다 명확하고 default initializer와 initializer inheritance의 이점을 챙길 수 있습니다.

### Customizing Init

init은 파라미터의 이름으로 구분됩니다. 

받는 파라미터의 수, 각각의 자료형이 모두 같더라도 파라미터의 이름을 다르게 해준다면 서로 다른 initializer로 동작합니다.

### default init, memberwise init

init을 구현하지 않아도 모든 값에 default 값이 있으면

default init을 사용할 수 있습니다.

구조체의 경우 default 값이 지정되지 않아도 memberwise init을 통해 

값을 지정하며 초기화할 수 있습니다.

class에는 이런 기능이 없는 이유는 상속이 존재해서입니다. (어떤 클래스를 상속하여 거기에 저장 프로퍼티를 추가한다면? memberwise init을 통해 초기화되어야 했던 여러 프로퍼티들이 초기화되지 않으면서 문제 발생 가능)



### Initializer Delegation

initializer는 또 다른 initializer를 호출할 수 있습니다

이를 통해 중복코드를 줄일 수 있습니다.

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



