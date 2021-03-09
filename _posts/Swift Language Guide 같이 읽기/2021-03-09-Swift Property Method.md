---
layout: post
title: "Swift Language Guide 같이 읽기 [Property 1편]"
image:
categories: [Swift Language Guide 같이 읽기]
tags: 
  - Swift
  - Property
  - Method
sitemap:
  changefreq: daily
  priority: 1.0
---

### 안녕하세요! Neph🌱입니다!! 😆😆

제 첫 시리즈물인 `Swift Language Guide 같이 읽기`의 첫 글을 `Property`로 시작해보려 합니다! 그 어떤 자료보다 강력한 [공식문서](https://docs.swift.org/swift-book/LanguageGuide/TheBasics.html)! 꼼꼼하게 한 단락 한 단락 이해해보면서 같이 읽어보자는 취지에서 기획한 시리즈입니다! 잘못된 점이 있다면 `댓글`로 알려주세요 😀



#### 오늘은 Swift Language Guide의 [Properties](https://docs.swift.org/swift-book/LanguageGuide/Properties.html) 와 함께합니다.



글을 시작하기 앞서 Swift에 존재하는 프로퍼티의 종류에 대해 먼저 계층도를 그려볼게요

- 프로퍼티 (Property)
  - 저장 프로퍼티 (Stored Property)
    - 지연 저장 프로퍼티 (Lazy Stored Properties)
  - 연산 프로퍼티 (Computed Property)
  - 타입 프로퍼티 (Type Property)

프로퍼티면 그냥 프로퍼티지.. 연산 프로퍼티? 타입 프로퍼티는 뭐지?

이런 의문점들을 공식문서를 정독하며 해결해보도록 합시다!



##### [Property 1편]에서는 저장 프로퍼티와 연산 프로퍼티에 대해,

##### [Property 2편]에서는 타입 프로퍼티와 프로퍼티 wrapper에 대해 다루어 볼 예정입니다.



### Properties

---

> 프로퍼티는 클래스, 구조체, 열거형(enumeration)과 연관된 값입니다. 저장 프로퍼티는 고정된 값(constant), 혹은 변하는(variable) 값을 instance로 가지는 반면, 연산 프로퍼티는 저장의 기능을 수행하기보단 연산의 기능을 수행하는 값입니다. 연산 프로퍼티는 클래스, 구조체, 열거형에 사용할 수 있으며, 저장 프로퍼티는 오직 클래스와 구조체에서만 사용할 수 있습니다.
>
> 저장 프로퍼티와 연산 프로퍼티는 주로 instance에 특정 타입으로 연결되어있지만, 타입 그 자체로도 존재할 수 있습니다. 이것이 바로 타입 프로퍼티입니다.
>
> 여러분은 프로퍼티 감시자를 통해 프로퍼티의 값의 변화를 감시(monitor)할 수 있습니다. 프로퍼티 감시자는 여러분이 만든 자신만의 프로퍼티에 적용할수도, 상위 클래스로부터 상속받은 서브 클래스의 프로퍼티에도 적용할 수 있습니다.
>
> 프로퍼티 wrapper를 통해 getter와 setter의 코드를 재활용할 수도 있습니다. 

---

첫 문단에서는 프로퍼티의 종류와 그 사용용도 등에 대해서 설명해주고 있습니다. 타입 자체가 프로퍼티로 있을 수 있다는 점, 프로퍼티의 변화를 감시하는 프로퍼티 감시자, Swift 5.1에 새롭게 등장한 프로퍼티 wrapper가 눈에 띄네요



### 저장 프로퍼티(Stored Properties)

---

> 가장 단순한 저장 프로퍼티의 역할은 상수나 변수를 특정 클래스나 구조체의 instance에 저장하는 것입니다. 저장 프로퍼티는 변수 저장 프로퍼티와 (var 키워드를 사용) 상수 저장 프로퍼티로 (let 키워드를 사용) 나눌 수 있습니다.
>
> 저장 프로퍼티에는 디폴트 값을 저장할 수 있으며 초기값을 초기화 과정중에(during initialization) 설정할 수도 있습니다. 
>
> 아래의 예시는 정수의 범위가 초기화 이후에 바뀔 수 없는 `FixedLengthRange`라는 구조체의 코드입니다.
>
> ```swift
> struct FixedLengthRange {
> var firstValue: Int
> let length: Int
> }
> var rangeOfThreeItems = FixedLengthRange(firstValue: 0, length: 3)
> // the range represents integer values 0, 1, and 2
> rangeOfThreeItems.firstValue = 6
> // the range now represents integer values 6, 7, and 8
> ```
>
> `FixedLengthRange`의 instance는 `firstValue`라는 변수 저장 프로퍼티와 `length`라는 상수 저장 프로퍼티를 가집니다. 상수 저장 프로퍼티로 만들어지는 `length`는 추후에 변경될 수 없습니다.
>
> #### 상수 구조체 instance의 저장 프로퍼티
>
> 만약 상수로 구조체 instance를 선언한다면 instance의 변수 프로퍼티도 변경할 수 없습니다.
>
> ```swift
> let rangeOfFourItems = FixedLengthRange(firstValue: 0, length: 4)
> // this range represents integer values 0, 1, 2, and 3
> rangeOfFourItems.firstValue = 6
> // this will report an error, even though firstValue is a variable property
> 
> ```
>
> `rangeOfFourItems`가 상수로 선언되었기 때문에 `firstValue`가 변수 프로퍼티임에도 불구하고 수정할 수 없습니다.
>
> 이런 상황은 구조체가 `value type`이기 때문에 벌어집니다. instance의 값 타입이 상수로 지정되면 그 instance의 모든 프로퍼티들도 이를 따라갑니다.
>
> 하지만 `reference type`인 클래스의 경우에는 이와 다른 양상을 보입니다. instance를 상수로 선언하더라도 변수 프로퍼티를 변수로써 사용할 수 있습니다.
>
> #### 지연 저장 프로퍼티
>
> 지연 저장 프로퍼티는 사용되기 전까지 초기값을 계산하지 않는 프로퍼티입니다. `lazy`키워드를 붙임으로써 지연 저장 프로퍼티를 사용할 수 있습니다.
>
> > 변수 프로퍼티에 대해서는 항상 `lazy`키워드를 붙여야 합니다. 초기값이 instance가 생성된 이후로(initialization 이후로) 사용되지 않을 수 있기 때문입니다. 하지만 상수 프로퍼티는 instance가 생성이 종료되기 전에 (before initialization complete) 값을 가지기 때문에 `lazy` 키워드를 사용할 수 없습니다.
>
> 지연 저장 프로퍼티는 프로퍼티의 초기값이 외부의 요소에 의해 의존하여 instance 생성 직후에 그 초기값을 알 수 없는 경우에 유용합니다. 또한 프로퍼티의 초기값을 구하기 위해 복잡한 연산이 필요한 경우, 초기값을 사용할 일이 생기기 전까지 이러한 복잡한 연산을 진행하지 않을 수 있다는 장점이 있습니다.
>
> 아래의 예시는 지연 저장 프로퍼티를 활용해 복잡한 클래스의 불필요한 초기화를 막는 과정입니다. 
>
> ```swift
> class DataImporter {
>     /*
>     DataImporter는 외부의 파일에서 데이터를 가져오는 클래스입니다.
>     초기화 과정에서 매우 많은 양의 정보를 가져온다고 가정합니다.
>     */
>     var filename = "data.txt"
>     // 이곳에 데이터를 가져오는 코드들이 위치합니다.
> }
> 
> class DataManager {
>     lazy var importer = DataImporter()
>     var data = [String]()
>     // 이곳에 데이터를 관리하는 코드들이 위치합니다.
> }
> 
> let manager = DataManager()
> manager.data.append("Some data")
> manager.data.append("Some more data")
> // DataImporter의 instance인 importer 프로퍼티가 아직 생성되지 않았습니다. (importer를 사용하지 않았기 때문)
> ```
>
> `DataManager`클래스는 `data`라는 저장 프로퍼티를 가지고 있습니다. 구체적인 코드의 구현이 위의 예시에 적혀있지 않지만 `DataManager`클래스가  `data`를 관리할 것임을 알 수 있습니다.
>
> `DataManager`의 기능 중 하나는 데이터를 파일로부터 가져오는 것입니다. 이 기능은 `DataImporter` 클래스를 통해 매우 많은 양의 정보를 가져오게 됩니다.
>
> `DataManager` instance가 데이터를 가져오지 않고 관리할 가능성이 있기 때문에 `DataImporter`클래스의 instance를 미리 생성해놓지 않습니다. (`lazy`키워드로 인해) 대신 처음으로 데이터를 불러오는 시점에 instance를 생성합니다.
>
> > 다만 다중 스레드 환경에서 지연 저장 프로퍼티에 동시다발적으로 접근하면 한 번만 초기화될거라는 보장이 없습니다.



저장 프로퍼티에 대해 다룬 단락이었습니다. 상수 구조체 instance의 프로퍼티가 모두 상수가 된다는 점, `lazy`키워드를 통한 지연 저장 프로퍼티의 활용 등이 눈에 띄는 단락이네요! 😀

다음으로는 연산 프로퍼티에 대해 다루어볼거에요! 저장 프로퍼티보다 신박한 내용들이 많으니 잘 따라와주세요 😝



### 연산 프로퍼티

---

> 저장 프로퍼티 외에도 클래스, 구조체, 열거형은 실제로 값을 저장하지 않는 연산 프로퍼티를 만들 수 있습니다. 다른 프로퍼티의 값을 간접적으로 이용하고 수정하기 위해 `getter`와 `setter`를 제공합니다. 
>
> ```swift
> struct Point {
>     var x = 0.0, y = 0.0
> }
> struct Size {
>     var width = 0.0, height = 0.0
> }
> struct Rect {
>     var origin = Point()
>     var size = Size()
>     var center: Point {
>         get {
>             let centerX = origin.x + (size.width / 2)
>             let centerY = origin.y + (size.height / 2)
>             return Point(x: centerX, y: centerY)
>         }
>         set(newCenter) {
>             origin.x = newCenter.x - (size.width / 2)
>             origin.y = newCenter.y - (size.height / 2)
>         }
>     }
> }
> var square = Rect(origin: Point(x: 0.0, y: 0.0),
>                   size: Size(width: 10.0, height: 10.0))
> let initialSquareCenter = square.center
> square.center = Point(x: 15.0, y: 15.0)
> print("square.origin is now at (\(square.origin.x), \(square.origin.y))")
> // Prints "square.origin is now at (10.0, 10.0)"
> ```
>
> 위 코드는 도형과 관련된 세가지 구조체가 정의되어있습니다.
>
> - `Point`는 



