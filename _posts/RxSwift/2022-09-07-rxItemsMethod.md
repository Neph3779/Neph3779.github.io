---
layout: post
title: "RxSwift items 메서드의 이해"
categories: [RxSwift]
tags: 
  - items
sitemap:
  changefreq: daily
  priority: 1.0
---

## items 메서드 구현부


```swift

public func items<Sequence: Swift.Sequence, Cell: UICollectionViewCell, Source: ObservableType>
        (cellIdentifier: String, cellType: Cell.Type = Cell.self)
        -> (_ source: Source)
        -> (_ configureCell: @escaping (Int, Sequence.Element, Cell) -> Void)
        -> Disposable where Source.Element == Sequence {
        return { source in
            return { configureCell in
                let dataSource = RxCollectionViewReactiveArrayDataSourceSequenceWrapper<Sequence> { cv, i, item in
                    let indexPath = IndexPath(item: i, section: 0)
                    let cell = cv.dequeueReusableCell(withReuseIdentifier: cellIdentifier, for: indexPath) as! Cell
                    configureCell(i, item, cell)
                    return cell
                }
                    
                return self.items(dataSource: dataSource)(source)
            }
        }
    }
```

items 메서드의 구현부인데 코드가 상당히 복잡합니다.

동작과정의 핵심은

- 클로저를 반환한다
  - 이 클로저는 인자를 받는다
- 반환된 클로저에 어떤 값을 넣는다
  - makeClosure라는 메서드가 Int 타입의 인자를 받는 클로저를 반환한다면
  - makeClosure()(1) 이러한 문장을 작성할 수 있는 것이다.



### 사용 예시

```swift
let numbers = Observable.just([1, 2, 3])

numbers
		.bind(to: collectionView.rx.items(cellIdentifier: "Cell", cellType: NumberCell.self)) { row, element, cell in
			cell.value?.text = "\(element) \(row)"
	}.disposed(by: disposeBag)
```



```swift
public func bind<R1, R2>(to binder: (Self) -> (R1) -> R2, curriedArgument: R1) -> R2 {
        binder(self)(curriedArgument)
    }
```



items메서드는 

- (cellIdentifier: String, cellType: Cell.Type = Cell.self)
- (_ source: Source)
- (_ configureCell: @escaping (Int, Sequence.Element, Cell) -> Void)
- Disposable where Source.Element == Sequence

위의 총 4가지의 과정을 거쳐 Disposable을 리턴합니다.



bind 메서드의 구현부를 보면 (Self) -> (R1) -> R2 타입의 인자를 받아서 R2를 리턴합니다.

자세한 동작원리에 대한 이해는 어려워도 bind가 리턴하는 것은 R2입니다.

즉, (_ configureCell: @escaping (Int, Sequence.Element, Cell) -> Void)를 리턴하는 것이죠

위의 클로저는 (Int, Sequence.Element, Cell) -> Void 타입의 escaping 클로저를 인자로 받고 있으므로

후행 클로저 문법을 적용하여 

```swift
{ row, element, cell in
			cell.value?.text = "\(element) \(row)"
	}
```

와 같은 블록을 뒤에 적어주면 되는 것이었습니다.



## 마무리

RxSwift를 공부하며 클로저를 자유자재로 반환하고 사용하면서 고도의 추상화를 해서 사용자가 쓸 수 있도록 만들어놨다는걸 매번 느끼는 것 같습니다. 동작원리에 대해 완벽하게 이해하기는 어렵겠지만 이 글 정도의 이해는 앞으로도 시도해볼 법하다 생각됩니다.
