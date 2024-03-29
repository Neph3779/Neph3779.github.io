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

associated value를 if let 구문을 통해 선언하다보니 좌변에 이를 적어주어야 하기 때문입니다.

~~if case let .category(category)를 적는 동안에 item의 type을 inference 할 수 없으므로~~

~~Switch문을 사용할때와는 다르게 자동완성 기능을 사용할 수 없습니다.~~

Xcode 14 업데이트 이후 자동완성 기능이 개선되어 자동완성으로 if case let 구문을 작성할 수 있는 것이 확인됐습니다!

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20221002173406.png" alt="image-20221002173400446" style="zoom:50%;" />

<br/>

```swift
// 됨
if case let .category(category) = itemWrapper {

}

// 안됨
if case let .category(let category) = itemWrapper {

}

// 안됨
else case let .product(product) = itemWrapper {
  
}

// 됨
else if case let .product(product) = itemWrapper {
  
}
```

switch 문을 사용할 때는 .category(let category)와 같이 연관값이 들어가는 자리에 변수, 상수를 선언하여 사용했지만

이미 let을 통해 상수임을 선언한 상태이기 때문에 중복으로 let을 쓰거나 var 키워드를 적어줄 수 없습니다. 



### if case let 구문을 사용하는 이유

Switch문을 사용하면 코드가 한 depth 더 깊어지게 되는데

만약 switch문 내에서 if let 구문을 통해 optional binding을 추가로 진행해야 한다면 코드를 한 depth를 늘리지 않고도 처리할 수 있다는 장점이 있습니다.

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