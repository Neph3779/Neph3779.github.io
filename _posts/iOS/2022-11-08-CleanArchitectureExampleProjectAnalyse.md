---
layout: post
title: "Clean Architecture 예제 프로젝트 분석 1 - Coordinator 패턴"
categories: [iOS]
tags: 
  - 
sitemap:
  changefreq: daily
  priority: 1.0
---

## Coordinator Pattern

> Coordinator: [한 계통의 각 움직임을] 조정[통합, 일원화]하다; [행동 등을] 조정하다

단어의 의미에서 알 수 있듯 Coordinator는 움직임을 조정하는 역할을 합니다.

<br/> 

Khanlou: Massive View Controller의 가장 큰 문제 중 하나는 flow logic 및 business logic이 얽혀있다는 것이다.

다음은 Khanlou가 예시로 든 Coordinator 패턴을 사용하지 않았을 때 발생하는 문제점을 설명할 코드입니다.

(출처: https://zeddios.medium.com/coordinator-pattern-bf4a1bc46930)

**example** 

```swift
func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
    let object = self.dataSource[indexPath]
    let detailViewController = SKDetailViewController(with: object)
    self.navigationController?.present(detailViewController, animated: true, completion: nil)
}
```



위의 예제에서 문제가 되는 부분은 다음에 보여질 화면을 만들고 (3번째 줄)

이를 부모 ViewController에게 이를 명령하고 있다는 부분입니다. (4번째 줄)

심지어 이 flow는 여러군데에 분산되어 있을 수 있으므로, 이 흐름들을 관리하는 것도 어렵습니다.

이를 Coordinator 프로토콜을 사용해 해결한 패턴이 바로 Coordinator Pattern입니다.

### Coordinator 계층도

![image-20221108190443802](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20221108190443.png)

```swift
final class AppCoordinator: Coordinator, SignUpCoordinatorDelegate {
  var navigationController: UINavigationController
  
  init(navigationController: UINavigationController) {
    self.navigationController = navigationController
  }
  
  func start() {
    if isNewUser {
      showSignUpView()
    } else {
      showMainView()
    }
  }
  
  func didSignUpped() {
    showMainView()
  }
  
  private func showMainView() {
    let coordinator = MainCoordinator(navigationController: navigationController, delegate: self)
    coordinator.start()
  }
  
  private func showSignUpView() {
    let coordinator = SignUpCoordinator(navigationController: navigationController, delegate: self)
    coordinator.start()
  }
}
```

**앱을 처음 사용하는 유저**는 `SignUpViewController`를 띄워주고,

이미 가입이 완료된 **기존 유저**에게는 `MainViewController`를 띄워주며

Main화면에서 **검색버튼을 누르면** `SearchViewController`를 띄워주는 

간단한 플로우의 앱이 있다고 가정해보겠습니다.

<br/> 

AppDelegate/SceneDelegate이 가지고 시작하는 AppCoordinator는 root Coordinator 역할을 합니다.

앱이 처음 실행되면 AppCoordinator 프로토콜의 start() 메서드를 호출하여

사용자가 신규유저인지 기존유저인지 체크한 뒤, 각 상황에 맞게 하위 Coordinator를 생성합니다.

<br/> 

```swift
protocol SignUpCoordinatorDelegate {
  func didSignUpped()
}

final class SignUpCoordinator: Coordinator, SignUpViewControllerDelegate {
  var navigationController: UINavigationController
  weak var delegate: SignUpCoordinatorDelegate?
  
  init(navigationController: UINavigationController, delegate: SignUpCoordinatorDelegate) {
    self.navigationController = navigationController
    self.delegate = delegate
  }
  
  func start() {
    let viewController = SignUpViewController(delegate: self)
    navigationController.pushViewController(viewController, animated: true)
  }
  
  func signUp() {
    self?.delegate.didSignUpped()
  }
}
```

최하위 Coordinator에서는 viewController를 만들고 이를 화면에 띄우는 작업을 진행합니다. 이때 delegate을 통해 이벤트를 상위 Coordinator에 전달합니다.

```swift
protocol SignUpViewControllerDelegate {
  func signUp()
}

final class SignUpViewController: UIViewController {
  weak var delegate: SignUpViewControllerDelegate?
  
  init(delegate: SignUpViewControllerDelegate) {
    self. delegate = delegate
  }
  
  @objc
  private func signUpButtonTapped() {
    delegate?.signUp()
  }
}
```

마지막으로 ViewController에서는 사용자로부터 전달받은 이벤트를 delegate를 통해 Coordinator에 전달합니다.

최종적으로 Coordinator의 flow는 다음과 같습니다.

![image-20221108204545895](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20221108204545.png)

### Coordinator 패턴을 써야하는 이유

화면전환 로직의 경우 비즈니스 로직임에도 불구하고 ViewController 내에서 처리했었기 때문에 역할이 분리되지 않았고 ViewController 자체를 재활용하기도 어려웠습니다. 

하지만 Coordinator 패턴을 사용함으로써 ViewController는 다른 ViewController에 대한 내용을 몰라도 될 뿐만 아니라

모든 화면전환 Flow들도 Coordinator가 Tree 형식으로 모아서 관리하게되므로 흐름을 파악하기 쉽고 관리가 쉽습니다.

<br/> 

> 본 글은 [해당글](https://zeddios.medium.com/coordinator-pattern-bf4a1bc46930)을 참조하여 적었습니다.