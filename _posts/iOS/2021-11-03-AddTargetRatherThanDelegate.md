---
layout: post
title: "간단한 Cell Touch 이벤트의 전달하기 (Delegate 대신 클로저 전달)"
categories: [iOS]
tags: 
  - cell touch event
  - addTarget
sitemap:
  changefreq: daily
  priority: 1.0
---

## 간단한 Cell Touch 이벤트는 클로저 전달로

이벤트를 전달하기 위해 대부분 Delegate Pattern을 사용해왔었다.

Delegate가 훌륭한 패턴이긴 하지만 손이 많이 가기 때문에 (protocol 제작, 이를 반영하는 작업 필요)

간단한 작업 정도는 클로저를 전달하여 사용하는 것도 괜찮다는 생각이 들어 이를 정리해보았다.



### 기존 Delegate Pattern

Cell에서 일어난 터치 이벤트를 ListViewController가 받아서 처리하는 것이기 때문에

ListViewController가 ListCellDelegate를 채택하며 (ListViewController가 ListViewCell의 일을 대신 처리해주는 대리자)

ListViewCell은 listViewCellDelegate를 weak var로 가지고 있으면서 필요할때 이벤트의 처리를 부탁한다.

```swift
protocol ListCellDelegate: AnyObject {
  func doSomething()
}

final class ListViewController: UIViewController {
  // ...
}

extension ListViewController: ListCellDelegate {
  func doSomething() {
    // cell에서 터치 이벤트가 발생했을때 이를 처리하는 코드
  }
}

final class ListViewCell: UITableViewCell {
  weak var listCellDelegate: ListCellDelegate? // 나(Cell)의 이벤트를 받아 처리해줄 대리자(ListCellDelegate를 채택한 ListViewController)
  
  private func cellDidTouchUp() {
    // ListViewController에서 해야하는 일을 delegate가 대신 해줌
    listCellDelegate?.doSomething()
  }
}
```



### 클로저를 전달하는 방법

cell에서 처리할 수 없는 경우에 이를 클로저 형태로 만들어 놓은뒤 클로저의 동작을 외부(ListViewController)에서 주입해주면 된다.

```swift
extension ListViewController: UITalbeViewDataSource {
  func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {

    cell.doSomething = {
      // cell의 터치 이벤트를 클로저로 지정
    }

    return cell
  }
}

final class ListViewCell: UITableViewCell {
  weak var doSomething: (() -> ())?
  
  // ...
  
  @objc func cellDidTouchUp() {
    doSomething?() // 외부(ListViewController)에서 넣어줌
  }
}
```

이렇게 간단한 경우에는 클로저를 전달하는 방식이 Delegate를 사용하는것보다 코드가 간결해지는 것을 알 수 있다.

