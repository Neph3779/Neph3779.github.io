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



## Section Provider

