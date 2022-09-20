---
layout: post
title: "if case let 구문은 뭐고 언제 쓰는걸까?"
categories: [Swift]
tags: 
  - if case let
sitemap:
  changefreq: daily
  priority: 1.0
---

## Swift에서 가장 독특한 문법(?)

if case let 구문은 어떤 associated value를 가진 enum값을 분기처리하는 구문입니다.

```swift
enum ItemWrapper: Hashable {
  case category(ProductCategory)
  case product(Product)
}

// ...

switch item {
  case .category(let category):
    // use category here
  case .product(let product):
    // use product here
}
```

같은 로직의 코드를 위처럼 switch로도 처리가 가능한데

Xcode의 자동완성 기능 덕분에 한번 써보면 아 이렇게 associtaed value를 쓰면 되는구나~

하고 감이 옵니다.

<br/> 

하지만 item이라는 ItemWrapper(enum) 타입의 변수가

어떤 case에 속하는지를 걸러내는 분기 코드를 switch가 아닌 if else 구문처럼 사용하고 싶다면

꽤나 독특한 방법으로 이용해야 합니다.

<br/> 

```swift
if case let .category(category) = item {
	// use category here
}
```

item이 ItemWrapper의 category라는 case인지에 대해서 물어봐야 하므로

if item == ... 와 같이 진행될거라 생각하기 쉽지만, 이 문법의 경우에는 그 반대로 적어야 합니다.

이유는 associated value 때문인데 if let 구문의 특성상 변수, 상수의 선언은 left hand side에 적혀져야 하나봅니다.

그러다보니 if case let .category(category)를 적는 동안에 Xcode는 어떠한 자동완성도 지원해줄 수 없습니다.

<br/> 

이 문법의 독특한 점은 여기서 끝이 아닌데

```swift
// 됨
if case let .category(category) = itemWrapper {

}

// 안됨
if case let .category(let category) = itemWrapper {

}

// 안됨
else case let .product(let product) = itemWrapper {
}
```

switch 문을 사용할 때는 .category(let category)와 같이 연관값이 들어가는 자리에 변수, 상수를 선언하여 사용했지만

이미 let을 통해 상수임을 선언한 상태이기 때문에 중복으로 let을 쓰거나 var 키워드를 적어줄 수 없습니다. 

또, else case let 같은 경우에는 사용할 수가 없습니다. 

> 이걸 언제 써요? 

Switch문을 사용하고 싶지 않을 때에 사용하면 됩니다.

Switch문을 사용하면 코드가 한 depth 더 깊어집니다.

만약 if let 구문으로 optional binding을 추가로 진행해야 한다면 코드를 한 depth 더 깊게 들어가지 않게 할 수 있습니다. 

### 사용 예제 코드

```swift
if case let .category(category) = itemWrapper,
   let categoryCell = collectionView
    .dequeueReusableCell(withReuseIdentifier: CategoryCell.reuseIdentifier,
                                         for: indexPath) as? CategoryCell {
    categoryCell.setUpContents(category: category)
    return categoryCell
}

if case let .product(product) = itemWrapper,
   let recommnedCell = collectionView
    .dequeueReusableCell(withReuseIdentifier: RecommendProductCell.reuseIdentifier,
                         for: indexPath) as? RecommendProductCell {
    if self.homeViewModel.isLoading {
        recommnedCell.showLoadingIndicator()
    } else {
        recommnedCell.setUpContents(product: product)
    }
    return recommnedCell
}
```

원래라면 switch문을 통해 case를 기준으로 분기한 뒤, 다시 if let을 내부에서 사용해 categoryCell을 얻었겠지만

이를 한번에 처리함으로써 가독성을 올릴 수 있었습니다.