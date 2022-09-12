---
layout: post
title: "UICollectionViewCompositionalLayout 정리 - 2"
categories: [iOS]
tags: 
  - Compositional Layout
  - collectionView
sitemap:
  changefreq: daily
  priority: 1.0
---

본 글은 [Getting Started with UICollectionViewCompositionalLayout](https://lickability.com/blog/getting-started-with-uicollectionviewcompositionallayout/)을 토대로 이해한 내용을 요약한 글입니다.

## Supplementary Items

### 기존의 header 구현

```swift
extension PhotosDataSource: UICollectionViewDataSource {
    func collectionView(_ collectionView: UICollectionView, viewForSupplementaryElementOfKind kind: String, at indexPath: IndexPath) -> UICollectionReusableView {
        guard let headerView = collectionView.dequeueReusableSupplementaryView(ofKind: kind, withReuseIdentifier: "HeaderSupplementaryView", for: indexPath) as? HeaderSupplementaryView else {
          return HeaderSupplementaryView()
        }
        
        headerView.viewModel = HeaderSupplementaryView.ViewModel(title: "Section \(indexPath.section + 1)")
        
        return headerView
    }
}
```

- 위와 같이 DataSrouce의 viewForSupplementaryElementOfKind 메서드를 통해 작업했음
- header의 크기와 같은 경우 headerReferenceSize 프로퍼티를 조절하거나 delegate 메서드를 통해 크기 지정했음



### Compositional Layout의 header 구현

- 기본적으로 SupplementaryView를 Datasource 메서드에 반환하는 것은 이전과 동일
- 크기 조정, 사용의 경우 아래의 예제처럼 진행

```swift
let headerItemSize = NSCollectionLayoutSize(widthDimension: .fractionalWidth(1),
                                            heightDimension: .estimated(100))
let headerItem = NSCollectionLayoutBoundarySupplementaryItem(layoutSize: headerItemSize,
                                                             elementKind: "header",
                                                             alignment: .top)
section.boundarySupplementaryItems = [headerItem]
```

- header의 size를 정할때도 NSCollectionLayoutSize를 사용
- NSCollectionLayoutBoundarySupplementaryItem은 NSCollectionLayoutSuuplementaryItem의 서브클래스로, header, footer를 작업할 수 있음



## Nested Groups

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220910221236.png" alt="Diagram of a compositional layout with multiple items and nested groups" style="zoom:50%;" />

- NSCollectionLayoutGroup 클래스는 NSCollectionLayoutItem의 서브클래스

- 즉 Section내에 들어가는 item으로 group 자체가 들어갈수도 있다.

  

```swift
let largeItemSize = NSCollectionLayoutSize(widthDimension: .fractionalWidth(0.5),
                                                   heightDimension: .fractionalHeight(1))
let largeItem = NSCollectionLayoutItem(layoutSize: largeItemSize)
largeItem.contentInsets = NSDirectionalEdgeInsets(top: inset, leading: inset,
                                                  bottom: inset, trailing: inset)

let smallItemSize = NSCollectionLayoutSize(widthDimension: .fractionalWidth(1),
                                           heightDimension: .fractionalHeight(0.5))
let smallItem = NSCollectionLayoutItem(layoutSize: smallItemSize)
smallItem.contentInsets = NSDirectionalEdgeInsets(top: inset, leading: inset,
                                                  bottom: inset, trailing: inset)

// Nested Group
let nestedGroupSize = NSCollectionLayoutSize(widthDimension: .fractionalWidth(0.25),
                                             heightDimension: .fractionalHeight(1))
let nestedGroup = NSCollectionLayoutGroup.vertical(layoutSize: nestedGroupSize,
                                                   subitems: [smallItem])

// Outer Group
let outerGroupSize = NSCollectionLayoutSize(widthDimension: .fractionalWidth(1),
                                            heightDimension: .fractionalWidth(0.5))
let outerGroup = NSCollectionLayoutGroup.horizontal(layoutSize: outerGroupSize,
                                                    subitems: [largeItem,
                                                               nestedGroup,
                                                               nestedGroup])

// Section
let section = NSCollectionLayoutSection(group: outerGroup)
section.contentInsets = NSDirectionalEdgeInsets(top: inset, leading: inset,
                                                bottom: inset, trailing: inset)
```



- subitems에 largeItem, nestedGroup, nestedGroup 를 넣어준 모습

> 만약 largeItem, nestedGroup 이렇게 두개만 넣어줬다면?

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20220910224745.png" alt="iPhone simulator screenshot showing compositional layout next to empty space" style="zoom: 25%;" />

- 이런식으로 남은 공간은 여백으로 채워짐



## 좌우 스크롤

```swift
section.orthogonalScrollingBehavior = .continuous
```

좌우 스크롤 가능 여부를 위와 같이 한줄로 처리할 수 있음

옵션으로는 

- none
- continuous
- continuousGroupLeadingBoundary
- paging
- groupPaging
- groupPagingCentered

가 있음 

