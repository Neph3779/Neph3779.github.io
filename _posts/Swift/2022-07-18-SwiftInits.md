---
layout: post
title: "Swift initializer 총정리"
image:
categories: Swift
tags: 
  - init
  - required init
  - convinience init
  - override init
sitemap:
  changefreq: daily
  priority: 1.0

---

이번 글에서는 [Swift.org 문서](https://docs.swift.org/swift-book/LanguageGuide/Initialization.html)를 바탕으로 Swift에서 사용되는 다양한 initializer의 종류와 그 사용처에 대해 알아보겠습니다.



## Initializers

initialize에 관한 기초적인 설명 파트는 제외하고 눈여겨 볼만한 내용들만 정리해보았습니다.

- Deinit은 class type의 경우에만 구현할 수 있습니다.
- 클래스와 구조체는 인스턴스 생성 시점에 반드시 모든 저장 프로퍼티에 값을 저장해야합니다.
- optional type의 값은 값 지정이 이루어지지 않는다면 nil값으로 초기화됩니다.
- 초기값은 init을 통해서 할당할수도 있고 정의부에서 default 값을 할당할수도 있습니다. (이때 전자와 후자의 경우 모두 프로퍼티 옵저버는 동작하지 않습니다.)

### Default value

만약 항상 같은 값으로 프로퍼티를 초기화해야 한다면 initializer를 통해서가 아닌 default value를 통해서 지정하길 권고하고 있습니다. 이 방법이 보다 명확하고 default initializer와 initializer inheritance의 이점을 챙길 수 있습니다.

### Customizing Init

init은 파라미터의 개수와 각각의 이름으로 구분됩니다.

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

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230620204134.png" alt="image-20230620204118609" style="zoom:50%;" />



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

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230620204152.png" alt="image-20230620204152034" style="zoom:50%;" />

- designated/convenience init이 클래스에서 호출됩니다.
- 클래스의 새 인스턴스를 위한 메모리 공간이 할당됩니다. (아직 메모리 공간이 초기화된 것은 아닙니다.)
- Designated initializer가 모든 저장 프로퍼티가 값을 가진 것을 확인하고 (해당 클래스의 것만, 부모 클래스의 것은 신경 X) 저장 프로퍼티의 메모리 공간을 initialize 합니다.
- Designated initializer가 상위 클래스에게 hands off 하고 상위 클래스의 init을 호출하여 같은 작업을 진행합니다.
- 이 작업은 체인의 top에 도달할때까지 진행됩니다.
- top 체인까지 도달했다면 마지막 클래스(최상위 부모 클래스)는 모든 저장 프로퍼티에 값이 들어간 것을 확인합니다. 인스턴스의 메모리 초기화 작업이 끝났다면 phase 1이 끝난 것입니다.



#### Phase 2

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230620204226.png" alt="image-20230620204226259" style="zoom:50%;" />

- 체인을 다시 타고 내려오면서 각 designated initializer는 인스턴스를 customize할 수 있는 기회를 부여받습니다. 이제 인스턴스는 self에 접근할 수 있으며 인스턴스 메서드를 호출할수도 있습니다.
- 최종적으로 convenience init이 customize의 기회를 부여받습니다. 이때 convenience init도 self를 사용할 수 있습니다. (당연하게도 convenience init을 통해 init하지 않았다면 이 과정은 생략됩니다.)



## Initializer 상속과 오버라이딩

Swift는 상위클래스의 initializer를 default로 상속받지 않습니다. 이는 상위 클래스로부터 받은 simple한 init이 더욱 specialized된 서브 클래스가 사용하게 되는 것을 막습니다. (subclass가 복잡한 처리과정을 통해 init되어야 하는데 이를 위해 만든 init이 아닌 상위클래스로부터 상속받은 init을 사용해버리면 의도와 다른 무의미한 instance가 생성될 가능성이 있고, 이를 방지하는 차원)

만약 서브클래스가 상위클래스와 같은 인터페이스의 init을 가지길 원한다면 Designated init의 경우 overriding을 통해 구현할 수 있습니다. 반면 상위 클래스의 Convenience init은 서브클래스가 직접 호출할 수 없으므로 overriding을 통해 구현하지 않습니다.

공식문서에서는 asynchronous하게 동작하는 super class의 init에 대해서도 다루고 있습니다. phase2에 subclass가 superclass의 프로퍼티를 customize하지 않는다면 이제 super.init()을 호출할 수 있는데, 이때 superclass의 init이 asynchronous하게 동작한다면 super.init() 앞에 await 키워드를 명시적으로 써주기를 요구하고 있습니다.



## Automatic Initializer Inheritance

서브클래스는 슈퍼클래스의 이니셜라이저를 디폴트로 상속받지 않지만 특정 상황이 맞아떨어진다면 automatic하게 상속받는 경우가 있습니다. 즉 서브클래스가 여러가지 시나리오에 대비하여 만들어진 이니셜라이저들을 모두 오버라이딩 할 필요가 없다는 것입니다. 

아래의 두가지 규칙을 만족시키면서 서브클래스에 추가된 새 프로퍼티들에 디폴트 값을 부여했다고 가정해봅시다.

1. 만약 서브클래스가 designated initializer를 정의하지 않았다면, 슈퍼클래스의 designated initializer를 자동으로 상속받습니다.
2. 만약 서브클래스가 모든 슈퍼클래스의 designated initializer를 구현 한다면 (super class의 모든 designated init을 오버라이딩 한다는 뜻, 규칙 1에 의해 상속받는 것도 구현으로 취급) 슈퍼클래스의 convenience init을 자동으로 상속받습니다.



## 자동 상속 예제 코드

```swift
class Food {
    var name: String
    init(name: String) {
        self.name = name
    }
    convenience init() {
        self.init(name: "[Unnamed]")
    }
}

class RecipeIngredient: Food {
    var quantity: Int
    init(name: String, quantity: Int) {
        self.quantity = quantity
        super.init(name: name)
    }
    override convenience init(name: String) {
        self.init(name: name, quantity: 1)
    }
}
```



위의 코드는 Food라는 클래스와 이를 상속받는 RecipeIngredient라는 클래스가 구현되어있습니다.

여기서 눈여겨 봐야 할 부분은 RecipeIngredient의 override convenience init(name: String) 부분인데

Food의 convenience init(unnamed: String)을 오버라이딩하는게 아닌것을 알아두어야 합니다.

super class(Food)의 designated init인 init(name: String)을 override하고 있으며 단지 이것을 convenience init으로 사용하고 있기에 convenience init이라고 적혀있는 것입니다. 

designated init을 오버라이딩함과 동시에 이를 convenience init으로도 활용할 수 있다는 점이 인상 깊네요.



이제 위에서 설명한 convenience init의 자동 상속에 대해 아래의 두 예시를 통해 살펴보겠습니다.

| ![image-20220720162637957](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220720162637.png) | ![image-20220720162631734](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220720162631.png) |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|                    자동상속이 이루어진 예                    |                자동상속이 이루어지지 않은 예                 |

Convenience init의 자동상속이 이루어진 경우를 보면 모든 designated init이 오버라이딩되어 구현된 것을 확인할 수 있습니다. 반면 오른쪽의 경우에는 init(some: Int)가 오버라이딩 되지 않았으므로 convenience init이 자동 상속되지 않은 것을 확인할 수 있습니다.

상속받은 convenience init은 슈퍼클래스의 convenience init과 똑같이 동작합니다. 하지만 delegate을 RecipeIngredient의 designated init에 한다는 것이 중요합니다. 이를 그림으로 나타내면 아래와 같게 됩니다.

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230620204325.png" alt="image-20230620204325331" style="zoom:50%;" />



delegate을 어디에 한다는 말의 뜻이 이해하기 어려울 것 같아 아래의 코드 실행화면도 첨부합니다.

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220720170048.png" alt="image-20220720170048957" style="zoom:67%;" />



a라는 인스턴스를 Food로부터 자동상속 받은 Food의 convenience init으로 초기화 하였음에도

RecipeIngredient 클래스의 convenience init이 실행되면서 `hello`가 찍혀있음을 확인할 수 있습니다.



## Required Initializers

Required Initializer는 모든 서브 클래스가 해당 init을 구현할 것을 강제합니다. 

만약 override를 하는 경우라도 override 키워드를 적지 않습니다.



## 클로저나 함수를 통한 프로퍼티 초기화

view 요소를 초기화하는 과정에서 정말 자주 쓰이는 부분입니다.

자세한 설명보다는 예제 코드를 통해 살펴보시는것이 좋을 것 같습니다.

```swift
struct Chessboard {
    let boardColors: [Bool] = {
        var temporaryBoard: [Bool] = []
        var isBlack = false
        for i in 1...8 {
            for j in 1...8 {
                temporaryBoard.append(isBlack)
                isBlack = !isBlack
            }
            isBlack = !isBlack
        }
        return temporaryBoard
    }()
    func squareIsBlackAt(row: Int, column: Int) -> Bool {
        return boardColors[(row * 8) + column]
    }
}
```

boardColors 프로퍼티를 보면 Boolean 배열인데 이를 초기화하는 과정이 마치 클로저처럼 적혀있는 것을 볼 수 있습니다.

클로저 반환값 유추가 아직 지원되고 있지 않기 때문에 (곧 지원 예정이라는 사실을 들은 적이 있네요)

반드시 해당 타입을 명시한 뒤에 클로저를 통해 초기화해주어야 합니다.



## Deinit

Deinitializer는 인스턴스가 메모리 해제되기 직전에 "자동으로" 호출됩니다.

이때 상속이 이루어졌다면 Superclass의 deinit도 순차적으로 타고 올라가며 지행됩니다.

(이 과정은 deinit을 직접 구현하지 않더라도, super.deinit을 호출하지 않더라도 진행됩니다.)

또한 deinit은 직접 호출할 수 없으며, deinit이 호출되는 시점은 인스턴스가 살아있는 시점이므로 인스턴스의 프로퍼티에 모두 접근할 수 있습니다.

## 마무리

Failable Initializer에 대한 설명의 경우 일반적으로 유추할 수 있는 내용이 많고 거의 사용되지 않으므로 필요한 경우 직접 공식문서를 참고해보시는게 좋을 것 같습니다.

이번 공부를 통해서 initializer가 어떤식으로 체인되어 올라가고, 또 왜 super.init()을 호출하기 전에 반드시 자신이 추가한 프로퍼티에 초기값을 꼭 넣어야 컴파일이 되는 것인지에 대해서도 이해할 수 있게 되었습니다.

override convenience init을 보아도 super class의 convenience init을 override한 것이 아닌 designate init을 override 하였지만 convenience init으로 사용한다는 것도 알게 되었습니다.