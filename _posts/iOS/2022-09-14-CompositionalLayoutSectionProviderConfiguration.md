---
layout: post
title: "Compositional Layout SectionProvider, Configuration"
categories: [iOS]
tags: 
  - collectionView
  - sectionProvider
sitemap:
  changefreq: daily
  priority: 1.0
---

## 상황, 위치에 따라 다른 Layout을 보여주기

- CollectionView를 구성하다보면 상황에 따라 cell이 보여지는 형태(layout)를 바꾸고 싶은 경우가 종종 있음
  - 또는 하나의 CollectionView내의 서로 다른 section에 다양한 형태의 cell들이 보여지게 하고 싶음
- 이를 기존 collectionView 구현에서는 flow layout delegate 메서드에서 분기처리를 통해 cell의 크기 지정 등의 방법을 통해 해결했음
- Compositional layout에서는 layout과 관련된 delegate를 사용하기 보단 NSCollectionView stuffs를 사용함
- UICollectionViewLayout을 만드는 방법은 4가지가 있음
  - 가장 기본적인 section을 인자로 주어 만드는 방법
  - section과 configuration을 주어 만드는 방법
  - sectionProvider를 인자로 주어 만드는 방법
  - sectionProvider와 configuration을 인자로 주어 만드는 방법
- 상황마다 다른 Layout을 구성하기 위해서 3, 4번 방법을 사용함



## SectionProvider

![image-20220914183720049](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220914183720.png)

- SectionProvider는 클로저를 typealias로 묶어둔 형태
- Int 인자는 section의 index를 나타내는 변수

아래와 같이 분기처리를 통해 각 section별로 다양한 layout을 만들어줄 수 있음

```swift
func createPerSectionLayout() -> UICollectionViewLayout {
    let layout = UICollectionViewCompositionalLayout { (sectionIndex: Int,
        layoutEnvironment: NSCollectionLayoutEnvironment) -> NSCollectionLayoutSection? in
        
        let columns = sectionIndex == 0 ? 2 : 4
        
        let itemSize = NSCollectionLayoutSize(widthDimension: .fractionalWidth(1.0),
                                             heightDimension: .fractionalHeight(1.0))
        let item = NSCollectionLayoutItem(layoutSize: itemSize)
        
        let groupSize = NSCollectionLayoutSize(widthDimension: .fractionalWidth(1.0),
                                              heightDimension: .absolute(44))
        let group = NSCollectionLayoutGroup.horizontal(layoutSize: groupSize,
                                                          subitem: item,
                                                            count: columns)
        
        let section = NSCollectionLayoutSection(group: group)
        return section
    }
    return layout
}
```



## LayoutEnvironment

위의 예제 코드에서 보이는 NSCollectionLayoutEnvironment는 traitCollection을 활용하여 

앱의 현재 traitCollection을 가지고 분기처리를 해줄 수 있음

traitCollection의 종류로는 다크모드 여부, 화면의 size class 등이 있음

```swift
let layout = UICollectionViewCompositionalLayout { (sectionIndex: Int,
    layoutEnvironment: NSCollectionLayoutEnvironment) -> NSCollectionLayoutSection in
        
    if layoutEnvironment.traitCollection.userInterfaceStyle == .dark {
        return sectionForUserInterfaceStyle(.dark)
    } else {
        return sectionForUserInterfaceStyle(.light)
    }
}
```



## LayoutConfiguration

![image-20220917183026986](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220917183027.png)

layout configuration은 configuration을 따로 빼서 작업할 수 있다는데 의의가 있음

```swift
let headerFooterSize = NSCollectionLayoutSize(widthDimension: .fractionalWidth(1.0),
                                             heightDimension: .estimated(44))

let header = NSCollectionLayoutBoundarySupplementaryItem(layoutSize: headerFooterSize,
                                                        elementKind: "header",
                                                          alignment: .top)
let footer = NSCollectionLayoutBoundarySupplementaryItem(layoutSize: headerFooterSize,
                                                        elementKind: "footer",
                                                          alignment: .bottom)

let config = UICollectionViewCompositionalLayoutConfiguration()
config.interSectionSpacing = 20
config.scrollDirection = .horizontal
config.boundarySupplementaryItems = [header, footer]
```

사용 예제가 사용법의 전부라.. 설명은 생략

configuration에서 설정할 수 있는 값도 너무 적어서 configuration을 통해 큰 이점을 가져가긴 어렵다고 생각됨

