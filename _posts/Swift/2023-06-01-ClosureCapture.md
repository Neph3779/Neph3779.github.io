---
layout: post
title: "Swift Closure Capture"
image:
categories: [Swift]
tags: 
  - Closure
  - Capture
sitemap:
  changefreq: daily
  priority: 1.0
---

## Closure Capture

클로저 내부에서 클로저 외부의 변수를 사용하게 되는 경우  클로저는 해당 변수를 "캡쳐"합니다.

클로저가 실행될 때 해당 시점에 변수에 저장된 값을 사용하게 되는 것과 같은 말입니다.



### Escaping closure

어떤 함수가 클로저를 인자로 받아 함수 내부에서 해당 클로저를 호출할 때

함수가 종료되기 전에 클로저의 실행이 이루어질 수도 있지만

비동기적으로 함수를 호출한다면 클로저의 실행보다 함수의 종료가 먼저 이루어질 수도 있습니다.

이렇게 함수가 종료된 후 실행될 예정인 클로저를 인자로 받을때는 `@escaping` 키워드를 클로저의 타입 앞에 붙여주어야 합니다.



### Capturing Values

함수가 종료된 후 실행되어야 하는 클로저가 내부에서 "함수" 내에 선언된 변수를 사용해야 한다면

스위프트는 클로저가 정상적으로 해당 변수를 사용할 수 있도록 해당 변수를 유지시킵니다.

만약 클로저의 실행이 끝났더라도 해당 클로저를 다시 실행시킬 수 있는 가능성이 있다면 **변수를 계속 유지**시킵니다.

내부적으로 어떻게 동작하는지까지는 알기 어렵겠지만, 클로저가 메모리에 존재하는 동안, 클로저가 해당 변수에 계속해서 접근할 수 있도록 변수를 메모리에 유지합니다.

클로저가 실행될때마다 특정 상태/값으로 초기화된 변수를 만들어 사용하는 것이 아닌 것에 유의해야 합니다.



### Weak / Unowned Capture

```swift
viewModel.reloadData = {
  self.collectionView.reloadData()
}
```

값을 캡쳐할때 위의 코드처럼 self를 사용한다면 self는 strong 참조된 것입니다.

만약 ViewController 내부에 위와 같은 클로저의 전달이 이루어졌다면 

ViewModel의 프로퍼티인 `reloadData`가 ViewController의 instance를 strong 참조하고

ViewController는 자기 자신의 프로퍼티인 ViewModel instance를 strong 참조하므로 

아래의 그림처럼 Strong retain cycle이 발생합니다.

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230601025740.png" alt="image-20230601025740092" style="zoom:50%;" />

이러한 순환참조를 방지하기 위해 대부분의 상황에서 클로저가 외부 변수를 사용해야 한다면 

해당 변수를 weak 참조하여 사용합니다.

```swift
viewModel.reloadData = { [weak self] in
  self?.collectionView.reloadData()
}
```

클로저의 인자, 리턴 타입을 명시하기 전에 대괄호를 열고 그 안에 참조할 방법(`weak`, `unowned`)를 적고 그 뒤에 변수를 적으면 해당 변수를 원하는 방법으로 참조하여 사용할 수 있습니다.

만약 여러개의 변수를 참조하고 싶다면

```swift
viewModel.reloadData = { [weak a, b, c unowned d] in
  // use a, b, c, d
}
```

위의 코드처럼 사용이 가능합니다.



### Closure inside Closure Capture

```swift
viewModel.reloadData = {
  DispatchQueue.main.async { [weak self] in
    self?.collectionView.reloadData()
  }
}
```

겉보기에 문제가 없을 것으로 보이는 위의 코드는 사실 memory leak을 유발합니다.

그 이유는 DispatchQueue.main.async 함수의 후행 클로저 내부에서 사용하는 self를 

자기 자신의 위치로부터 가장 가까이에 있는 곳으로부터 가져오기 때문입니다.

즉, viewModel.reloadData 클로저로부터 self를 가져오게 되며, 이 곳에서의 self를 weak 참조한다고 명시해놓지 않았기에 묵시적으로 self를 강한참조하여 가져오게 됩니다.

이미 strong 참조된 self를 weak으로 참조해봤자 의미가 없는 것이죠.

```swift
viewModel.reloadData = { [weak self] in
  DispatchQueue.main.async { 
    self?.collectionView.reloadData()
  }
}
```

설명한 내용을 이해했다면 자연스럽게 위의 코드는 메모리 누수가 없이 동작할 것임을 알 수 있습니다.

(DispatchQueue 블록 내부에서는 self를 weak 참조하지 않았는데도 말이죠!)



### inout 변수의 Capture

```swift
  func reduce(into state: inout State, action: Action) -> EffectTask<Action> {
    switch action {
    case .decrementButtonTapped:
      state.count -= 1
      return .none

    case .incrementButtonTapped:
      state.count += 1
      return .run { [count = state.count] send in
        let (data, _) = try await URLSession.shared
          .data(from: URL(string: "http://numbersapi.com/\(count)")!)
        let fact = String(decoding: data, as: UTF8.self)
      }
    }
  }
```

inout 변수의 경우 ecaping closure에서 사용하기 위해선 명시적인 캡쳐가 이루어져야 합니다.

이때 inout 변수 자체를 캡쳐하고자 한다면 대괄호 안에 해당 변수를 다시 적기만 해도 되지만

inout 변수의 특정 필드값을 골라 캡쳐하고자 한다면 위의 코드처럼 변수명을 지정해주어야 합니다.

