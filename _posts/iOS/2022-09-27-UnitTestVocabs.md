---
layout: post
title: "Unit Test에 사용되는 용어들과 사용법"
categories: [iOS]
tags: 
  - TDD
  - Unit test
sitemap:
  changefreq: daily
  priority: 1.0
---

본 글은 Saad El Oulladi의 [Unit Tests (Swift): Mocking the right way.](https://saad-eloulladi.medium.com/unit-tests-swift-mocking-the-right-way-65488848a3fc)를 읽고 작성된 글입니다.



## Unit Test 용어의 역사

2001년 Tim Mackinnon, Steven Freeman, Philip Craig는 unit test와 관련된 글을 작성했습니다. 그 이름은 "Endo-testing: unit testing with mock objects".

Mock이란 용어는 이곳에서 처음 사용되었으며 "의존성 문제를 해결하기 위한 가짜이자 실제 객체에게서 기대한 모든 역할을 수행하는 요소"이란 뜻으로 사용되었습니다.

2007년 Gerard Meszaros는 Test double을 "결과적으로 객체의 의존성 문제를 해결하기 위해 사용되는 모든 요소"라고 말했습니다.

Meszaros는 너무 많은 unit test 관련 용어들이 개발자 커뮤니티 사이에서 불명확한 의미를 지닌채 사용되고 있으며,  같은 의미를 지녔음에도 서로 다른 용어로 쓰이거나 반대로 다른 의미를 지녔음에도 서로 같은 용어로 쓰이는 경우가 잦다고 주장했습니다.

그는 여러 타입의 test double들을 나타내기 위한 어휘들을 제안했고 이 어휘들은 오늘날 unit test 관련 용어의 표준이 되었습니다.



### 용어의 중요성

test double의 특성에 맞는 단어를 사용해야지만 어떤 작업을 한 것인지 이해하기 쉬우며 convention을 지키기 쉽습니다.

unit test를 위해 test double을 고를때에도 어떤 test double을 선택해야 적합한지에 대해 따지기도 쉬워집니다.



> **SUT :** "System under test"의 약자로, 현재 테스트 중인 요소를 나타냅니다. ex) 현재 테스트중인 class



## Dummy

Dummy는 단순히 컴파일러가 에러를 내지 않도록 의존성을 주입시켜주는 요소입니다. 아무런 기능을 하지 않으며 nil값을 전달하는 경우도 많습니다. 또한 해당 클래스를 확실히 쓰지 않는다는 사실을 명확히 해주기도 합니다.

Dummy를 만들기 위해 프레임워크나 툴이 사용되지 않는 것이 대부분입니다. (빈 구현부를 만들어주는 것이 다이므로)

**사용 예시**

```swift
// Logger interface definition
protocol Logger {
    func log(message: String)
}

// Logger real implementation
class PrintLogger: Logger {

    func log(message: String) {
        print(message)
    }
}

class NetworkManager {
  private let logger: Logger
  
  init(logger: Logger) {
    self.logger = logger
  }
}

// Dummy logger will only sit there to fill the dependency
class DummyLogger: Logger {
    
    // Will do nothing.
    func log(message: String) {
        // Nothing ...
    }
}
```

NetworkManager의 기능만을 테스트하고 싶더라도 NetworkManager는 Logger가 있어야지만 생성할 수 있습니다.

이때 Logger는 NetworkManager의 테스트동안 아무 작업도 하지 않는다는 사실을 "DummyLogger"라는 빈 객체를 만들어 보여줄 수 있습니다.



## Stub

Stub는 아마 test double중 가장 흔히 쓰이는 케이스일겁니다. Stub는 SUT가 unit test를 진행하는동안 input으로 받는 인자의 이상적인 값을 제공합니다. 

Stub는 클래스와 같은 툴을 통해 생성할수도 있고 직접 필요한 값들을 하드코딩 값으로 채워넣을 수도 있습니다.

Stub는 간접적으로 제공되는 값에 대한 제어가 필요할 때 사용됩니다. 테스트 할 객체가 Verification을 입증하는 값이 필요하다고 할때 이 값을 Verification 로직 없이 제공하는 것이 바로 Stub입니다.



**사용 예시**

```swift
// The account manager dependency interface.
protocol AccountManagable {
    var isUserAuthenticated: Bool { get }
}

class NetworkManager {
  private let accountManager: AccountManagable
  func checkUserAuthentication() {
    if !accountManager.isUserAuthenticated {
      // ...
    }
  }
}

class AccountManagerStub: AccountManagable {
    
    // We implement the dependency property.
    var isUserAuthenticated = true
}
```



NetworkManager는 계정의 authentication을 위해 Account Manager의 isUserAuthenticated 프로퍼티를 사용합니다. Account Manager의 실제 구현부에는 isUserAuthenticated를 설정하기 위한 로직들이 존재하겠지만 Network Manager를 테스트하는 과정에선 이를 Stub를 사용하여 로직 없이 해결할 수 있습니다.



## Spy

Spy는 조금 특별한 test double입니다. 언제, 어디서 호출되었는지를 기록하며 SUT에서 직접적으로 얻을 수 없는 정보들을 제공합니다.

Spy는 stub처럼 하드코딩된 값을 반환할수도 있지만 stub와는 다르게 단순히 간접적인 output들을 제공하는 역할만 한다는 것에서 차이가 납니다.



**사용 예시**

```swift
protocol EMailService {
    func send(email: String)
}

class OrderController {

    let emailService: EMailService
    
    init(emailService: EMailService) {
        self.emailService = emailService
    }
    
    // Order product method will call a send method wich
    // returns nothing and, hence it will be dificult to test that sent was called.
    func order(product: String) {
        emailService.send(email: "You orderd some \(product)")
    }
}

class EMailServiceSpy: EMailService {
    
    // In addition to the interface implementation.
    // The spy has some extra properties that
    // will remember how the dependency was called.
    var sendEmailCallCount: Int = 0
    
    func send(email: String) {
        sendEmailCallCount += sendEmailCallCount
    }
}
```



OrderController의 order 메서드를 보면 내부에서 send(email: String)를 호출하고 있습니다. send 메서드가 몇회 호출되었는지를 알아내기 위해 Spy가 사용됩니다.



## Mock

Spy와 마찬가지로 mock은 어떻게 호출되었는지에 대해 기록합니다. 하지만 spy가 테스트에 필요한 값을 기록하기만 했다면 mock은 verify 작업까지 진행하여 해당 값이 기대와 맞아떨어지는지를 체크합니다.



```swift
protocol EMailService {
    func send(email: String)
}

class OrderController {

    let emailService: EMailService
    
    init(emailService: EMailService) {
        self.emailService = emailService
    }
    
    // Order product method will call a send method wich
    // returns nothing and, hence it will be dificult to test that sent was called.
    func order(product: String) {
        emailService.send(email: "You orderd some \(product)")
    }
}

// A mock knows what is the right behavior that should take place.
class EMailServiceMock: EMailService {
    
    var sendEmailCallCount: Int = 0
    var emailUsageLimitation: Int = 10000

    // It can do a part of the verification that used to be done only inside the test.
    func verifySendEmailWasCalled() -> Bool {
        return sendEmailCallCount > 0
    }
    
    func isMailUsageExceedsLimit() -> Bool {
        return sendEmailCallCount > emailUsageLimitation
    }
    
    func send(email: String) {
        sendEmailCallCount += sendEmailCallCount
    }
}
```



위의 예시에서는 Mock객체가 단순히 emial 송신 횟수만을 알아내는 것이 아니라 email이 전송되었는지 여부를 체크하거나 송신 가능한 이메일의 제한을 초과하지 않았는지 등을 체크합니다.



### Mock v.s. Spy

일반적인 상황에서 Spy보단 Mock을 사용하는 것이 바람직합니다. Spy는 mock을 사용할 수 없을정도로 로직이 복잡하거나 코드의 코어부분을 testable하게 만들기 위해 리팩토링이 필요할 때 사용합니다.

혹은 mocking할 수 없는 프레임워크의 콜백함수를 SUT가 인자로 사용할때도 사용할 수 있습니다.



## Fake

Fake는 복잡한 의존관계를 단순화한 것입니다. 고정적인 값을 통해 대체될 수 없는 경우에 사용됩니다.

Fake는 로직들을 가지고 있는 test double입니다. 데이터베이스와 관련된 의존관계가 있다고 가정할 때 실제 데이터베이스를 테스트에서 사용할 수는 없으니 Fake 데이터베이스를 생성하여 테스트를 진행할 수 있습니다.

```swift
class FakeUserDefaults: UserDefaults {

    // A dictionary we will use to store during uni tests
    private var fakeStorage: [String: Any] = [:]
    
    override func object(forKey defaultName: String) -> Any? {
            return fakeStorage[defaultName]
    }
    
    // Override the real set method to fake user default behavior
    override func set(_ value: Any?, forKey defaultName: String) {
            fakeStorage[defaultName] = value
    }
}
```

예시를 보면 UserDefaults를 서브클래싱하여 필요한 로직을 override를 통해 테스트용으로 덮어씌운 것을 확인할 수 있습니다.

Fake 로직은 실제 객체와 별개로 성장할 수 있습니다. 이는 fake를 관리할 수 있도록 TDD를 진행할 것이 추천되는 이유이기도 합니다.



## 마무리

이번 글에서는 Unit test에서 사용되는 용어들의 뜻과 그 역할들을 사용 예시와 함께 알아보았습니다.

야곰 아카데미에서 처음 이 글을 보았을 때는 감이 전혀 오질 않고 글이 너무 길게만 느껴졌었는데

지금와서 다시 읽어보니 정말 잘 정리된 글이라는 생각이 들어서 블로그에도 기록을 남깁니다.